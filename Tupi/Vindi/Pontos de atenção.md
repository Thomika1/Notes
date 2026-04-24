- disputa manual deveria ser um dropdown para selecioanr adquirente
- deveria ter como selecionar merchant com dropdown
- rever tudo de cadastro de merchant
  
  ### 1. MerchantAcquirerIntegration (Agora o "Grupo/PV")

Esta tabela deixa de ser um "detalhe" do merchant e passa a ser a **Entidade de Conexão**. Ela deve conter o `AcquirerMerchantIdentifier` (o PV) que será usado pelo seu Service de coleta.

```
package business

import "github.com/tupi-fintech/chargeback-api/pkg/core/crud"
import enums "github.com/tupi-fintech/chargeback-api/pkg/domains/shared/enums/acquirer"

type MerchantAcquirerIntegration struct {
    crud.BaseOrganizationModel // Mudou para BaseOrganizationModel para ter isolamento por Org
    Name                       string             `json:"name" gorm:"type:varchar(255);not null"` // Nome amigável do "Grupo/PV"
    AcquirerName               enums.AcquirerName `json:"acquirerName" gorm:"type:varchar(255);not null"`
    AcquirerMerchantIdentifier string             `json:"acquirerMerchantIdentifier" gorm:"type:varchar(255);not null;uniqueIndex"`
    IsActive                   bool               `json:"isActive" gorm:"default:true;not null"`
    
    // Relacionamento: Um PV (Integração) agrupa vários Merchants
    Merchants []Merchant `json:"merchants,omitempty" gorm:"many2many:merchant_integration_items;"`
}
```

---

### 2. Merchant (O Lojista)

O `Merchant` agora se livra do `MerchantGroupID` e da `AcquirerMerchantIdentifier` direta (que você já suspeitava ser redundante).

```
type MerchantData struct {
    MerchantAssociation
    Address      *bases.Address `json:"address,omitempty" gorm:"embedded;embeddedPrefix:address_"`
    Contact      *bases.Contact `json:"contact,omitempty" gorm:"embedded;embeddedPrefix:contact_"`
    TaxID        string         `json:"taxId,omitempty" gorm:"type:varchar(255)"`
    LegalName    string         `json:"legalName,omitempty" gorm:"type:varchar(255)"`
    DBAName      string         `json:"dbaName,omitempty" gorm:"type:varchar(255)"`
    IsSpecial    *bool          `json:"isSpecial,omitempty" gorm:"type:bool"`
}

type MerchantAssociation struct {
    Partner            *Partner                      `json:"partner,omitempty" gorm:"foreignKey:MerchantID;references:ID"`
    // O Merchant agora aponta para suas integrações (PVs) via Many-to-Many
    AcquirerIntegrations []MerchantAcquirerIntegration `json:"acquirerIntegrations,omitempty" gorm:"many2many:merchant_integration_items;"`
    MerchantCategories   []MerchantCategoryAssociation `json:"merchantCategories,omitempty" gorm:"foreignKey:MerchantID;references:ID"`
    Transactions         []Transaction                 `json:"transactions,omitempty" gorm:"foreignKey:MerchantID;references:ID"`
}
```

---

### Por que isso funciona melhor?

- **Fim da Redundância:** Você não precisa mais gerenciar um `MerchantGroup` e uma `Integration` separadamente. O "Grupo" que o usuário vê no front-end é, tecnicamente, o **PV configurado**.
    
- **Suporte a Multi-Adquirente:** Como usamos `many2many`, um único `Merchant` (CNPJ) pode estar vinculado a um PV da **Rede** e a outro da **Cielo** simultaneamente, resolvendo o problema que você citou.
    
- **Performance na Cron:** O seu Job agora simplifica o loop:
    
    1. Busca todas as `MerchantAcquirerIntegration` ativas.
        
    2. Para cada uma, pega o `AcquirerMerchantIdentifier` e faz **uma única chamada** de coleta.
        
    3. Distribui os chargebacks recebidos entre os `Merchants` associados àquela integração.
        

### O que você "perde" (e como lidar):

Se o cliente precisar agrupar lojistas por um motivo **não técnico** (ex: "Lojas do Sudeste" vs "Lojas do Nordeste") e esses lojistas estiverem no mesmo PV, você não conseguirá separar usando apenas o PV. Se esse caso de uso não existir hoje na **Tupi**, a sua simplificação é o caminho mais inteligente para o código.

