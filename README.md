https://docs.fenergox.com/developer-hub/integration-flows/flow-initiators/external-data-adapter?utm_source=chatgpt.com


function transform(input: InputObject): ResultObject {
  const companies = [
    {
      Id: "MOCK-001",
      entityType: "Company",
      fullName: "Cedarstone Technologies LLC",
      matchHint: "Exact",
      matchScore: 99,
      data: {
        ExternalDataIdentifier: "MOCK-001",
        LegalEntityName: "Cedarstone Technologies LLC",
        CountryOfIncorporation: "USA"
      }
    },
    {
      Id: "MOCK-002",
      entityType: "Company",
      fullName: "Cedarstone Holdings Inc",
      matchHint: "Partial",
      matchScore: 85,
      data: {
        ExternalDataIdentifier: "MOCK-002",
        LegalEntityName: "Cedarstone Holdings Inc",
        CountryOfIncorporation: "USA"
      }
    }
  ];

  const criteria =
    input.request?.Request?.SearchCriteria || {};

  const name = String(
    criteria.LegalEntityName || ""
  ).toLowerCase().trim();

  const results = name
    ? companies.filter(c =>
        c.fullName.toLowerCase().includes(name)
      )
    : companies;

  return {
    output: {
      response: {
        results
      }
    }
  };
}


--------


function transform(input: InputObject): ResultObject {
  const req = input.request;

  const field = (name: string, label: string) => ({
    Name: name,
    FriendlyName: label,
    Type: "text"
  });

  const adapterSchema = {
    Lookups: [],
    Schema: {
      SearchRequest: {
        SearchCriteria: {
          Fields: [
            field("LegalEntityName", "Legal Entity Name"),
            field(
              "CountryOfIncorporation",
              "Country of Incorporation"
            )
          ]
        }
      },

      SearchResponse: {
        SearchResultsData: {
          Fields: [
            field(
              "ExternalDataIdentifier",
              "External Data Identifier"
            ),
            field("LegalEntityName", "Legal Entity Name"),
            field(
              "CountryOfIncorporation",
              "Country of Incorporation"
            ),
            field("Address", "Address")
          ]
        }
      },

      GetRequest: {
        RequestData: {
          Fields: [
            field(
              "ExternalDataIdentifier",
              "External Data Identifier"
            )
          ]
        }
      },

      GetResponse: {
        ClientData: {
          Fields: [
            field(
              "ExternalDataIdentifier",
              "External Data Identifier"
            ),
            field("LegalEntityName", "Legal Entity Name"),
            field(
              "CountryOfIncorporation",
              "Country of Incorporation"
            ),
            field("RegistrationNumber", "Registration Number"),
            field("Website", "Website"),
            field("Address", "Address")
          ]
        },

        AssociatedEntitiesCompanyData: {
          Fields: []
        },

        AssociatedEntitiesIndividualData: {
          Fields: []
        },

        AssociationsData: {
          Fields: []
        }
      }
    }
  };

  return {
    output: {
      Id: req.Id,
      Tenant: req.Tenant,
      ProviderId: req.ProviderId,
      type: "Test",
      status: "Success",
      errorDetails: null,
      response: {
        adapterSchema
      }
    }
  };
}


-------------------------




function transform(input: InputObject): ResultObject {
  const req = input.request;

  // Mock company data
  const companies = [
    {
      Id: "MOCK-001",
      LegalEntityName: "Cedarstone Technologies LLC",
      CountryOfIncorporation: "USA",
      Address: "100 Example Road, Austin, TX"
    },
    {
      Id: "MOCK-002",
      LegalEntityName: "Cedarstone Holdings Inc",
      CountryOfIncorporation: "USA",
      Address: "200 Sample Avenue, Dallas, TX"
    }
  ];

  const criteria = req.Request?.SearchCriteria || {};

  const searchName = String(
    criteria.LegalEntityName || ""
  ).trim().toLowerCase();

  const searchCountry = String(
    criteria.CountryOfIncorporation || ""
  ).trim().toLowerCase();

  // Filter mock records
  const matches = companies.filter(company => {
    const nameMatches =
      !searchName ||
      company.LegalEntityName.toLowerCase()
        .includes(searchName);

    const countryMatches =
      !searchCountry ||
      company.CountryOfIncorporation.toLowerCase() ===
        searchCountry;

    return nameMatches && countryMatches;
  });

  // Convert matches into Fenergo Search Results
  const results = matches.map(company => {
    const exactMatch =
      searchName.length > 0 &&
      company.LegalEntityName.toLowerCase() === searchName;

    return {
      Id: company.Id,
      entityType: "Company",
      fullName: company.LegalEntityName,
      matchHint: exactMatch ? "Exact" : "Partial",
      matchScore: exactMatch ? 100 : 85,
      data: {
        ExternalDataIdentifier: company.Id,
        LegalEntityName: company.LegalEntityName,
        CountryOfIncorporation:
          company.CountryOfIncorporation,
        Address: company.Address
      }
    };
  });

  return {
    output: {
      Id: req.Id,
      Tenant: req.Tenant,
      ProviderId: req.ProviderId,
      type: "Search",
      status: "Success",
      errorDetails: null,
      response: {
        results
      }
    }
  };
}


-------------------------


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
