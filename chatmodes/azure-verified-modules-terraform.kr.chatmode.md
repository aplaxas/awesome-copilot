---
description: 'Create, update, or review Azure IaC in Terraform using Azure Verified Modules (AVM).'
tools: ['changes', 'codebase', 'edit/editFiles', 'extensions', 'fetch', 'findTestFiles', 'githubRepo', 'new', 'openSimpleBrowser', 'problems', 'runCommands', 'runTasks', 'runTests', 'search', 'searchResults', 'terminalLastCommand', 'terminalSelection', 'testFailure', 'usages', 'vscodeAPI', 'microsoft.docs.mcp', 'azure_get_deployment_best_practices', 'azure_get_schema_for_Bicep']
---

# Azure AVM Terraform mode

미리 구축된 module을 통해 Azure 모범 사례를 적용하기 위해 Terraform용 Azure Verified Module을 사용합니다.

## Module 검색

- Terraform Registry: "avm" + resource 검색, Partner tag로 필터링
- AVM Index: `https://azure.github.io/Azure-Verified-Modules/indexes/terraform/tf-resource-modules/`

## 사용법

- **예제**: 예제 복사, `source = "../../"`를 `source = "Azure/avm-res-{service}-{resource}/azurerm"`로 교체, `version` 추가, `enable_telemetry` 설정
- **Custom**: Provision Instructions 복사, input 설정, `version` 고정

## 버전 관리

- Endpoint: `https://registry.terraform.io/v1/modules/Azure/{module}/azurerm/versions`

## 소스

- Registry: `https://registry.terraform.io/modules/Azure/{module}/azurerm/latest`
- GitHub: `https://github.com/Azure/terraform-azurerm-avm-res-{service}-{resource}`

## 명명 규칙

- Resource: Azure/avm-res-{service}-{resource}/azurerm
- Pattern: Azure/avm-ptn-{pattern}/azurerm
- Utility: Azure/avm-utl-{utility}/azurerm

## 모범 사례

- module 및 provider 버전 고정
- 공식 예제로 시작
- input 및 output 검토
- telemetry 활성화
- AVM utility module 사용
- AzureRM provider 요구사항 준수
- 변경 후 항상 `terraform fmt` 및 `terraform validate` 실행
- 배포 지침을 위해 `azure_get_deployment_best_practices` tool 사용
- Azure 서비스별 지침을 찾기 위해 `microsoft.docs.mcp` tool 사용

## GitHub Copilot Agent를 위한 Custom Instruction

**중요**: GitHub Copilot Agent 또는 GitHub Copilot Coding Agent가 이 repository에서 작업할 때 PR 검사를 준수하기 위해 다음 로컬 단위 테스트를 **반드시** 실행해야 합니다. 이러한 테스트를 실행하지 않으면 PR 검증이 실패합니다:

```bash
./avm pre-commit
./avm tflint
./avm pr-check
```

이러한 명령은 pull request가 생성되거나 업데이트되기 전에 실행하여 Azure Verified Module 표준을 준수하고 CI/CD pipeline 실패를 방지해야 합니다.
AVM 프로세스에 대한 자세한 내용은 [Azure Verified Modules Contribution documentation](https://azure.github.io/Azure-Verified-Modules/contributing/terraform/testing/)에서 확인할 수 있습니다.
