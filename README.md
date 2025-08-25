# PropertApi
ASP.NET Core Web API for property analytics over Azure Synapse (parcels, buildings, companies, people, zoning, facts, and valuations).
---

## 📖 Overview
This project implements the **test task**, demonstrating solution design, data ingestion, transformation, API development, and deployment on Azure.

The solution integrates **geospatial datasets** (parcels, buildings, zoning, companies, people) with **Azure cloud services** to deliver a secure, scalable, and performant property data platform.

---

## Architecture
The architecture consists of:

1. **Data Ingestion (Azure Data Factory)**  
   - Ingests multi-format datasets (CSV, XLSX, XML, GeoJSON, SQL).  
   - Handles incremental loads with watermarking.  
   - Stores raw → processed data in **Azure Data Lake**.

2. **Data Transformation (Synapse/SQL Pools)**  
   - Cleans, enriches, and models data into fact/dimension schema.  
   - Supports queries for ownership, zoning, transactions, and surveillance events.

3. **API (ASP.NET Core 8 + EF Core)**  
   - Provides REST endpoints for property, company, and person queries.  
   - Features filtering, pagination, sorting, and aggregations.  
   - Secured with **Azure Entra ID (OIDC)** and **RBAC** roles.

4. **Secrets & Security (Azure Key Vault)**  
   - Connection strings and API keys managed securely.  
   - API retrieves secrets dynamically.

5. **Extras**  
   - CI/CD pipelines using GitHub Actions (API & ADF).  
   - Caching with **Azure Redis** for hot endpoints.  
   - Automated infrastructure with **Bicep templates**.

---

## Datasets
Six datasets were prepared with **100+ sample records each**:

1. **Land Parcels**  
   - Area, owner, cadastral codes, intended use, zoning.  

2. **Buildings**  
   - Property ID, floors, year built, regulatory status.  

3. **Zoning**  
   - Land use categories, restrictions, regulatory notes.  

4. **Companies**  
   - Business ID, board members, registration details.  

5. **People**  
   - Owners, shareholders, representatives linked to properties/companies.

Formats included: **CSV, XLSX, XML, GeoJSON, SQL**.  
📂 See `/datasets/` or `datasets.zip`
---

## ⚙️ API Setup

The **Property API** is built with **.NET 8 Web API** and provides endpoints for querying parcels, buildings, zoning, companies, people, and surveillance data.  
It integrates with **SQL Server**, **Azure AD (JWT bearer authentication)**, and **Redis caching**.

### 📂 Project Initialization
```bash
cd Projects/dotnet/easyeiendom/
dotnet new webapi -n PropertyApi
cd PropertyApi/

# Data access
dotnet add package Dapper
dotnet add package Microsoft.Data.SqlClient

# API documentation
dotnet add package Swashbuckle.AspNetCore

# Authentication & Identity
dotnet add package Azure.Identity
dotnet add package Microsoft.Identity.Web
dotnet add package Microsoft.AspNetCore.Authentication.JwtBearer
dotnet add package Microsoft.Identity.Web.MicrosoftGraph --version 2.*   # optional Graph integration

# Caching
dotnet add package StackExchange.Redis

dotnet dev-certs https --trust

# Standard run
dotnet run

# Restore packages then run
dotnet restore
dotnet run

# Hot reload
dotnet watch run

# Run on specific port
dotnet run --urls "https://localhost:7139"

# Free port 5101
fuser -n tcp -k 5101

# Free port 7139
fuser -n tcp -k 7139

Accessing the API

Once running, open in browser:

Swagger UI → https://localhost:7139/swagger

API health check → GET /api/health (default)

