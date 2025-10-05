# WSO2 API Validation Rules

This directory contains sample validation rules for APIs deployed to WSO2 APIM Manager, using Spectral rulesets.

## Overview

The rules are designed to validate WSO2 API configurations (metadata) and OpenAPI definitions.

## Rule Files

### basicValidation.yaml

This file contains general OpenAPI specification validation rules based on Adidas API guidelines. These rules ensure that OpenAPI specifications follow best practices for naming conventions, protocol usage, and content types.

**Documentation URL**: https://github.com/adidas/api-guidelines/blob/master/ruleset.md

#### General Rules (OAS v2.0 & v3.0)

##### adidas-paths-kebab-case
- **Description**: All YAML/JSON paths MUST follow kebab-case
- **Severity**: warn
- **Target**: `$.paths[*]~`
- **Validation**: Path segments must use kebab-case format (lowercase with hyphens)

##### adidas-path-parameters-camelCase-alphanumeric
- **Description**: Path parameters MUST follow camelCase
- **Severity**: warn
- **Target**: `$..parameters[?(@.in == 'path')].name`
- **Validation**: Path parameter names must be camelCase alphanumeric

##### adidas-definitions-camelCase-alphanumeric
- **Description**: All YAML/JSON definitions MUST follow fields-camelCase and be ASCII alphanumeric characters or `_` or `$`
- **Severity**: error
- **Target**: `$.definitions[*]~`
- **Validation**: Definition names must follow camelCase conventions

##### adidas-properties-camelCase-alphanumeric
- **Description**: All JSON Schema properties MUST follow fields-camelCase and be ASCII alphanumeric characters or `_` or `$`
- **Severity**: error
- **Target**: `$.definitions..properties[*]~`
- **Validation**: Property names must follow camelCase conventions

##### adidas-request-GET-no-body
- **Description**: A 'GET' request MUST NOT accept a 'body' parameter
- **Severity**: error
- **Target**: `$.paths..get.parameters..in`
- **Validation**: GET requests cannot have body parameters

##### adidas-headers-no-x-headers
- **Description**: All 'HTTP' headers SHOULD NOT include 'X-' headers (RFC 6648)
- **Severity**: warn
- **Target**: `$..parameters[?(@.in == 'header')].name`
- **Validation**: Headers should not use deprecated 'X-' prefix

##### adidas-headers-hyphenated-pascal-case
- **Description**: All HTTP headers MUST use Hyphenated-Pascal-Case notation
- **Severity**: error
- **Target**: `$..parameters[?(@.in == 'header')].name`
- **Validation**: Header names must follow Hyphenated-Pascal-Case format

#### OAS v2.0 Specific Rules

##### adidas-oas2-protocol-https-only
- **Description**: ALL requests MUST go through https protocol only
- **Severity**: error
- **Target**: `$.schemes`
- **Validation**: Only HTTPS protocol is allowed in schemes array

##### adidas-oas2-request-support-json
- **Description**: Every request SHOULD support application/json media type
- **Severity**: warn
- **Target**: `$..consumes`
- **Validation**: Requests should include application/json in consumes array

#### OAS v3.0 Specific Rules

##### adidas-oas3-request-support-json
- **Description**: Every request MUST support application/json media type
- **Severity**: error
- **Target**: `$.paths..requests.requestBody.content`
- **Validation**: Request bodies must support application/json content type

##### adidas-oas3-protocol-https-only
- **Description**: ALL requests MUST go through https protocol only
- **Severity**: error
- **Target**: `$.servers..url`
- **Validation**: Server URLs must use HTTPS protocol

##### adidas-oas3-response-success-hal
- **Description**: All success responses MUST be of media type application/hal+json
- **Severity**: error
- **Target**: Success response content (300, 304, 201)
- **Validation**: Success responses must use HAL JSON format

##### adidas-oas3-response-success-OK
- **Description**: All success responses MUST be of media type application/hal+json or application/problem+json
- **Severity**: error
- **Target**: OK response content (200, 202)
- **Validation**: OK responses must use HAL JSON or Problem JSON format

### policy-validation.yaml

This file contains validation rules for WSO2 API policies, ensuring that critical security and functionality policies like PIIMaskingRegex and SemanticPromptGuardrail are properly configured.

#### Policy Validation Rules

##### wso2-api-pii-masking-required
- **Description**: PIIMaskingRegex policy must be present at least one operation level
- **Severity**: error
- **Target**: `$.data.operations[*].operationPolicies.request[*]`
- **Validation**: Ensures PII masking protection is configured

##### wso2-api-semantic-guardrail-required
- **Description**: SemanticPromptGuardrail or SemanticCache policy must be present at operation level
- **Severity**: error
- **Target**: `$.data.operations[*].operationPolicies.request[*].policyName`
- **Validation**: Ensures semantic guardrails are configured for AI operations

##### wso2-api-pii-masking-has-entities
- **Description**: PIIMaskingRegex policy must have piiEntities parameter
- **Severity**: error
- **Target**: `$.data.operations[*].operationPolicies.request[*]`
- **Validation**: Validates PII masking configuration completeness

##### wso2-api-pii-masking-has-jsonpath
- **Description**: PIIMaskingRegex policy must have jsonPath parameter
- **Severity**: error
- **Target**: `$.data.operations[*].operationPolicies.request[*]`
- **Validation**: Ensures JSONPath targeting is configured for PII masking

##### wso2-api-semantic-guardrail-has-jsonpath
- **Description**: Semantic guardrail policies must have jsonPath parameter
- **Severity**: error
- **Target**: `$.data.operations[*].operationPolicies.request[*]`
- **Validation**: Ensures JSONPath targeting is configured for semantic guardrails

##### wso2-api-post-operations-pii-protected
- **Description**: POST operations should have PII masking protection
- **Severity**: warn
- **Target**: `$.data.operations[*]`
- **Validation**: Recommends PII protection for data submission operations

##### wso2-api-chat-operations-semantic-protected
- **Description**: Chat and completion operations should have semantic guardrails
- **Severity**: warn
- **Target**: `$.data.operations[*]`
- **Validation**: Recommends semantic protection for AI/ML operations

##### wso2-api-pii-redaction-enabled
- **Description**: PII masking should have redaction enabled for data protection
- **Severity**: warn
- **Target**: `$.data.operations[*].operationPolicies.request[*]`
- **Validation**: Recommends enabling redaction for sensitive data

##### wso2-api-operations-have-security-policy
- **Description**: Each operation should have at least one security policy
- **Severity**: error
- **Target**: `$.data.operations[*].operationPolicies.request`
- **Validation**: Ensures security measures are in place for all operations

##### wso2-api-pii-entities-structure
- **Description**: PII entities should be properly structured JSON
- **Severity**: error
- **Target**: `$.data.operations[*].operationPolicies.request[*]`
- **Validation**: Validates PII entity configuration structure

##### wso2-api-regex-guardrail-pattern
- **Description**: Regex guardrail policies should have non-empty regex patterns
- **Severity**: error
- **Target**: `$.data.operations[*].operationPolicies.request[*]`
- **Validation**: Ensures regex patterns are properly defined

### propertiesValidationRules.yml

This file contains validation rules for WSO2 API additional properties.

#### Rules Defined

##### wso2-api-additional-properties-required
- **Description**: WSO2 API must have additionalProperties defined
- **Severity**: error
- **Target**: `$.data`
- **Validation**: Ensures the `additionalProperties` field exists and is truthy

##### wso2-api-domain-valid-values
- **Description**: Domain property must have valid values: DevOps, IoT, Data, Security
- **Severity**: error
- **Target**: `$.data.additionalPropertiesMap.Domain`
- **Validation**: Domain value must match one of: `DevOps`, `IoT`, `Data`, `Security`

##### wso2-api-owner-valid-values
- **Description**: Owner property must have valid values: BU1, BU2, BU3, Global
- **Severity**: error
- **Target**: `$.data.additionalPropertiesMap.Owner`
- **Validation**: Owner value must match one of: `BU1`, `BU2`, `BU3`, `Global`

##### wso2-api-domain-required
- **Description**: Domain property is required in additionalPropertiesMap
- **Severity**: error
- **Target**: `$.data.additionalPropertiesMap`
- **Validation**: Ensures the `Domain` field exists and is truthy

##### wso2-api-owner-required
- **Description**: Owner property is required in additionalPropertiesMap
- **Severity**: error
- **Target**: `$.data.additionalPropertiesMap`
- **Validation**: Ensures the `Owner` field exists and is truthy

##### wso2-api-unknown-properties
- **Description**: Unknown properties found - only Domain and Owner are expected
- **Severity**: warn
- **Target**: `$.data.additionalProperties[*]`
- **Validation**: Warns if properties other than `Domain` and `Owner` are found

## Rule Structure

Each rule follows the Spectral rule format:

```yaml
rule-name:
  description: "Human-readable description of what the rule validates"
  given: "JSONPath expression targeting the data to validate"
  severity: error|warn|info|hint
  then:
    field: "field to validate within the given path"
    function: "validation function to apply"
    functionOptions: # optional parameters for the function
      option: value
```

## Usage

These rules are designed to be used with Spectral CLI tools to validate WSO2 API configuration, as well as within the WSO2 API Manager governance center. 
