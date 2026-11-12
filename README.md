
function transform(input: InputObject): ResultObject {
  const req = input.request;

  return {
    output: {
      Id: req.Id,
      Tenant: req.Tenant,
      ProviderId: req.ProviderId,
      type: "Search",
      status: "Success",
      errorDetails: null,

      response: {
        results: [
          {
            id: "POC-COMPANY-001",
            entityType: "Company",
            fullName: "Cedarstone",
            matchHint: "Exact",
            matchScore: 100,
            data: {
              ExternalDataIdentifier: "POC-COMPANY-001",
              LegalEntityName: "Cedarstone"
            }
          },
          {
            id: "POC-COMPANY-002",
            entityType: "Company",
            fullName: "POC Mock Company Two",
            matchHint: "Partial",
            matchScore: 85,
            data: {
              ExternalDataIdentifier: "POC-COMPANY-002",
              LegalEntityName: "POC Mock Company Two"
            }
          }
        ]
      }
    }
  };
}
