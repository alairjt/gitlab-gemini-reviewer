# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [0.2.0] - 2025-07-23

### Added
- Suporte para verificação de discussões existentes no GitLab MR
- Lógica para evitar a criação de discussões duplicadas
- Funcionalidade para marcar automaticamente como resolvidas as discussões cujos problemas foram corrigidos
- Tratamento de erros aprimorado para as chamadas à API do GitLab
- Verificação de discussões resolvíveis antes de tentar resolvê-las

### Changed
- Atualizado o método `resolve_discussion` para usar a nova API do GitLab
- Melhorada a extração de posições das notas de discussão
- Filtragem de issues por severidade antes de criar discussões

### Fixed
- Corrigido o tratamento de discussões sem posição
- Corrigida a verificação de discussões já resolvidas
- Melhorado o mapeamento de linhas para discussões

## [0.1.4] - 2025-07-23

### Added
- Versão inicial do revisor de código baseado em Gemini para GitLab MRs
- Suporte a revisão de código em português e inglês
- Integração com Jira para rastreamento de issues
- Geração de relatórios em formato JSON
- Suporte a configuração via variáveis de ambiente

[Unreleased]: https://github.com/alairjt/gitlab-gemini-reviewer/compare/v0.2.0...HEAD
[0.2.0]: https://github.com/alairjt/gitlab-gemini-reviewer/compare/v0.1.4...v0.2.0
[0.1.4]: https://github.com/alairjt/gitlab-gemini-reviewer/releases/tag/v0.1.4
