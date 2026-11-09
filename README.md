
function transform(input: InputObject): ResultObject {
  const req = input.request;

  const companyId = String(
    req.Request?.Id || ""
  ).trim();

  // Mock company data
  const companies = [
    {
      Id: "MOCK-001",
      LegalEntityName: "Cedarstone Technologies LLC",
      CountryOfIncorporation: "USA",
      RegistrationNumber: "TX-100100",
      Website: "https://example.com/cedarstone-tech",
      Address: "100 Example Road, Austin, TX"
    },
    {
      Id: "MOCK-002",
      LegalEntityName: "Cedarstone Holdings Inc",
      CountryOfIncorporation: "USA",
      RegistrationNumber: "TX-200200",
      Website: "https://example.com/cedarstone-holdings",
      Address: "200 Sample Avenue, Dallas, TX"
    }
  ];

  const company = companies.find(
    c => c.Id === companyId
  );

  if (!company) {
    throw new Error(
      "Mock company not found: " + companyId
    );
  }

  return {
    output: {
      Id: req.Id,
      Tenant: req.Tenant,
      ProviderId: req.ProviderId,
      type: "Get",
      status: "Success",
      errorDetails: null,
      response: {
        documents: null,

        client: {
          Id: company.Id,
          entityType: "Company",
          fullName: company.LegalEntityName,
          data: {
            ExternalDataIdentifier: company.Id,
            LegalEntityName: company.LegalEntityName,
            CountryOfIncorporation:
              company.CountryOfIncorporation,
            RegistrationNumber:
              company.RegistrationNumber,
            Website: company.Website,
            Address: company.Address
          }
        },

        hierarchy: {
          associatedEntities: [],
          associations: []
        }
      }
    }
  };
}




