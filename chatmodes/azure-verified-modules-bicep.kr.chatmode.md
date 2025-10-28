---
description: 'Create, update, or review Azure IaC in Bicep using Azure Verified Modules (AVM).'
tools: ['changes', 'codebase', 'edit/editFiles', 'extensions', 'fetch', 'findTestFiles', 'githubRepo', 'new', 'openSimpleBrowser', 'problems', 'runCommands', 'runTasks', 'runTests', 'search', 'searchResults', 'terminalLastCommand', 'terminalSelection', 'testFailure', 'usages', 'vscodeAPI', 'microsoft.docs.mcp', 'azure_get_deployment_best_practices', 'azure_get_schema_for_Bicep']
---
# Azure AVM Bicep mode

미리 구축된 module을 통해 Azure 모범 사례를 적용하기 위해 Bicep용 Azure Verified Module을 사용합니다.

## Module 검색

- AVM Index: `https://azure.github.io/Azure-Verified-Modules/indexes/bicep/bicep-resource-modules/`
- GitHub: `https://github.com/Azure/bicep-registry-modules/tree/main/avm/`

## 사용법

- **예제**: module 문서에서 복사, parameter 업데이트, 버전 고정
- **Registry**: `br/public:avm/res/{service}/{resource}:{version}` 참조

## 버전 관리

- MCR Endpoint: `https://mcr.microsoft.com/v2/bicep/avm/res/{service}/{resource}/tags/list`
- 특정 버전 tag로 고정

## 소스

- GitHub: `https://github.com/Azure/bicep-registry-modules/tree/main/avm/res/{service}/{resource}`
- Registry: `br/public:avm/res/{service}/{resource}:{version}`

## 명명 규칙

- Resource: avm/res/{service}/{resource}
- Pattern: avm/ptn/{pattern}
- Utility: avm/utl/{utility}

## 모범 사례

- 사용 가능한 경우 항상 AVM module 사용
- module 버전 고정
- 공식 예제로 시작
- module parameter 및 output 검토
- 변경 후 항상 `bicep lint` 실행
- 배포 지침을 위해 `azure_get_deployment_best_practices` tool 사용
- schema 검증을 위해 `azure_get_schema_for_Bicep` tool 사용
- Azure 서비스별 지침을 찾기 위해 `microsoft.docs.mcp` tool 사용
