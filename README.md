# Demo Schemas for Kafka Schema Registry MCP

🎨 **Realistic business schemas for comprehensive demo scenarios**

This repository contains carefully crafted Avro schemas that demonstrate real-world use cases for the [Kafka Schema Registry MCP Server](https://github.com/kafka-schema-reg-mcp-demos/kafka-schema-reg-mcp). Each schema showcases different aspects of schema design, evolution, and compatibility.

## 📁 Repository Structure

```
demo-schemas/
├── ecommerce/           # E-commerce platform schemas
│   ├── user-profile/    # User management and preferences
│   ├── order-events/    # Order lifecycle events
│   ├── product-catalog/ # Product information
│   └── recommendation/  # ML-driven recommendations
├── iot-platform/        # IoT and sensor data schemas
│   ├── sensor-readings/ # Environmental sensor data
│   ├── device-status/   # Device health and connectivity
│   └── alert-events/    # Threshold violations and alerts
├── fintech/            # Financial services schemas
│   ├── transactions/   # Payment and transfer events
│   ├── accounts/       # Account information
│   ├── compliance/     # Regulatory and audit data
│   └── risk-assessment/# Fraud detection and scoring
├── saas-platform/      # Multi-tenant SaaS schemas
│   ├── user-events/    # Customer activity tracking
│   ├── analytics/      # Business intelligence data
│   └── webhooks/       # Integration payloads
└── evolution-examples/ # Schema compatibility examples
    ├── backward/       # Backward compatible changes
    ├── forward/        # Forward compatible changes
    └── full/          # Fully compatible changes
```

## 🎯 Demo Use Cases

### 1. E-commerce Platform Evolution
**Business Context**: Growing online retailer expanding globally

- **User Profile**: Customer data with GDPR compliance evolution
- **Order Events**: Order lifecycle with payment integration
- **Product Catalog**: Inventory management with internationalization
- **Recommendations**: ML-driven personalization data

### 2. IoT Platform Scaling
**Business Context**: Smart city infrastructure monitoring

- **Sensor Readings**: Multi-sensor environmental data
- **Device Status**: Fleet management and maintenance
- **Alert Events**: Real-time threshold monitoring

### 3. Financial Services Compliance
**Business Context**: Digital bank with regulatory requirements

- **Transactions**: Payment processing with audit trails
- **Accounts**: Customer account management
- **Compliance**: Regulatory reporting and KYC
- **Risk Assessment**: Fraud detection and prevention

### 4. Multi-Tenant SaaS Growth
**Business Context**: B2B SaaS platform serving diverse customers

- **User Events**: Activity tracking per tenant
- **Analytics**: Business intelligence per customer
- **Webhooks**: Customer integration endpoints

## 🔄 Schema Evolution Examples

### Backward Compatible Changes ✅
- Adding optional fields with defaults
- Adding new enum values (at the end)
- Removing default values from optional fields

### Forward Compatible Changes ✅
- Removing optional fields
- Adding aliases to fields
- Changing field order

### Breaking Changes ❌
- Removing required fields
- Changing field types
- Renaming fields without aliases
- Changing enum values

## 🚀 Quick Start

### Browse Schemas
```bash
# Clone the schemas repository
git clone https://github.com/kafka-schema-reg-mcp-demos/demo-schemas.git
cd demo-schemas

# Explore schema categories
ls -la */

# View a specific schema
cat ecommerce/user-profile/v1.avsc
```

### Load into Demo Environment
```bash
# Clone the demo deployment repository
git clone https://github.com/kafka-schema-reg-mcp-demos/demo-deployment.git
cd demo-deployment

# Set up the complete demo environment with GitHub OAuth
./scripts/setup-github-oauth.sh

# The setup script automatically:
# 1. Starts all Schema Registry instances
# 2. Configures GitHub OAuth
# 3. Loads all demo schemas via setup-demo-data.sh
# 4. Sets up monitoring and UI
```

### Manual Schema Loading (Alternative)
```bash
# If you want to load schemas manually after environment is running
cd demo-deployment

# Ensure registries are running
docker-compose -f docker-compose.github-oauth.yml up -d

# Load schemas into all registries
./scripts/setup-demo-data.sh
```

### Test Schema Evolution
```bash
# Get a GitHub token first (for authentication)
export GITHUB_TOKEN="your_github_token_here"

# Register initial schema
curl -X POST http://localhost:38000/schemas \
  -H "Authorization: Bearer $GITHUB_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "registry": "development",
    "context": "ecommerce", 
    "subject": "user-profile",
    "schema_definition": '$(cat ecommerce/user-profile/v1.avsc)'
  }'

# Test compatibility with evolution
curl -X POST http://localhost:38000/compatibility \
  -H "Authorization: Bearer $GITHUB_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "registry": "development",
    "context": "ecommerce",
    "subject": "user-profile", 
    "schema_definition": '$(cat ecommerce/user-profile/v2.avsc)'
  }'
```

## 📊 Schema Metrics

| Domain | Schemas | Versions | Evolution Examples |
|--------|---------|----------|-------------------|
| E-commerce | 4 | 12 | GDPR compliance, internationalization |
| IoT Platform | 3 | 8 | Sensor additions, alert enhancements |
| Fintech | 4 | 10 | Regulatory changes, fraud detection |
| SaaS Platform | 3 | 9 | Multi-tenancy, analytics expansion |
| **Total** | **14** | **39** | **15 compatibility scenarios** |

## 🎨 Schema Design Principles

### 1. **Real-World Modeling**
- Based on actual business requirements
- Includes edge cases and optional fields
- Considers regulatory and compliance needs

### 2. **Evolution-Friendly Design**
- Optional fields with sensible defaults
- Extensible enum types
- Nested record structures for grouping

### 3. **Documentation Rich**
- Comprehensive field documentation
- Business context in schema descriptions
- Evolution rationale in version notes

### 4. **Type Safety**
- Appropriate Avro types for each use case
- Union types for optional complex fields
- Logical types for dates, timestamps, decimals

## 🔧 Integration with MCP Server

### Schema Registration
```bash
# Register with context organization
curl -X POST http://localhost:38000/schemas \
  -H "Authorization: Bearer $GITHUB_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "registry": "development",
    "context": "ecommerce",
    "subject": "user-profile",
    "schema_definition": {...}
  }'
```

### Compatibility Testing
```bash
# Test backward compatibility
curl -X POST http://localhost:38000/compatibility \
  -H "Authorization: Bearer $GITHUB_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "registry": "development",
    "context": "ecommerce",
    "subject": "user-profile",
    "schema_definition": {...}
  }'
```

### Cross-Registry Migration
```bash
# Promote from dev to staging
curl -X POST http://localhost:38000/migrate \
  -H "Authorization: Bearer $GITHUB_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "source_registry": "development",
    "target_registry": "staging",
    "subject": "user-profile",
    "source_context": "ecommerce",
    "target_context": "ecommerce"
  }'
```

## 🎯 Demo Environment Features

After running the setup, you'll have:

### 🏗️ **Multi-Registry Architecture**
- **Development**: Full read/write access, latest schema versions
- **Staging**: Limited write access, promoted schemas for testing  
- **Production**: Read-only access, stable production schemas

### 📊 **Schema Distribution**
- **Progressive deployment**: Schemas flow from dev → staging → production
- **Version management**: Different versions in each environment
- **Context organization**: Domain-based schema grouping

### 🔐 **GitHub OAuth Integration**
- **Organization-based access**: Authenticate via GitHub organization
- **Team-based permissions**: Different access levels per team
- **Real OAuth flows**: Production-ready authentication

### 📱 **Web Interface**
- **Schema browser**: Explore schemas via web UI at http://localhost:3000
- **Registry comparison**: View differences between environments
- **Interactive testing**: Test compatibility directly in the browser

## 📚 Learning Resources

### Schema Evolution Guides
- [Backward Compatibility](evolution-examples/backward/README.md)
- [Forward Compatibility](evolution-examples/forward/README.md)
- [Full Compatibility](evolution-examples/full/README.md)
- [Breaking Changes](evolution-examples/breaking/README.md)

### Business Domain Examples
- [E-commerce Schema Design](ecommerce/README.md)
- [IoT Platform Patterns](iot-platform/README.md)
- [Financial Services Compliance](fintech/README.md)
- [Multi-Tenant SaaS Architecture](saas-platform/README.md)

## 🤝 Contributing

We welcome contributions of new schemas and evolution examples!

### Adding New Schemas
1. Create domain directory following existing structure
2. Include v1.avsc and v1.json (with metadata)
3. Add evolution examples (v2, v3, etc.)
4. Document business context in README.md
5. Test compatibility scenarios

### Schema Guidelines
- Use descriptive names and documentation
- Follow Avro best practices
- Include realistic sample data
- Demonstrate evolution patterns
- Consider cross-domain relationships

## 🔗 Related Projects

- **[Main MCP Server](https://github.com/kafka-schema-reg-mcp-demos/kafka-schema-reg-mcp)** - Core MCP implementation
- **[Demo Deployment](https://github.com/kafka-schema-reg-mcp-demos/demo-deployment)** - Infrastructure and setup
- **[Demo Documentation](https://github.com/kafka-schema-reg-mcp-demos/demo-docs)** - Guides and tutorials

## 📄 License

These demo schemas are provided under the [MIT License](LICENSE) for educational and demonstration purposes.

---

🎯 **Ready to explore real-world schema management?** Start with the [demo deployment](https://github.com/kafka-schema-reg-mcp-demos/demo-deployment) and load these schemas!