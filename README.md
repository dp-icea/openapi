# Especificações OpenAPI do BR-UTM

Repositório oficial das especificações OpenAPI públicas do BR-UTM.

## Especificações

| Arquivo                                                            | API                                         | Quem implementa | Quem chama             |
| ------------------------------------------------------------------ | ------------------------------------------- | --------------- | ---------------------- |
| [`token.yaml`](token.yaml)                                         | Token API (Auth Server)                     | DECEA           | Todos os participantes |
| [`dss.yaml`](dss.yaml)                                             | DSS (Discovery and Synchronization Service) | DECEA           | SW-USS                 |
| [`sw-uss.yaml`](sw-uss.yaml)                                       | SW-USS (Gerenciador de Operações)           | Cada SW-USS     | Outros SW-USS          |
| [`sw-uss-homologacao.yaml`](sw-uss-homologacao.yaml)               | SW-USS: interfaces de homologação           | Cada SW-USS     | BR-UTM Lab (DECEA)     |
| [`gerenciador-constraints.yaml`](gerenciador-constraints.yaml)     | SW-USS Gerenciador de Constraints           | DECEA           | SW-USS                 |
| [`provedor-zona-utm.yaml`](provedor-zona-utm.yaml)                 | Provedor de Zona UTM                        | DECEA           | SW-USS                 |
| [`provedor-altitude-terreno.yaml`](provedor-altitude-terreno.yaml) | Provedor de Altitude de Terreno             | DECEA           | SW-USS                 |

As specs citam umas às outras pelo nome do arquivo (por exemplo, "ver `token.yaml`"). Por isso, os arquivos ficam todos na raiz e **não devem ser renomeados** sem uma versão major.

## Versionamento

O conjunto de specs é versionado com [SemVer](https://semver.org/lang/pt-BR/) por meio de **tags git** (`vMAJOR.MINOR.PATCH`). Uma tag congela todas as specs naquele estado; o histórico de cada versão fica no [CHANGELOG](CHANGELOG.md).

| Mudança                 | Exemplo                                                                                           | Incremento |
| ----------------------- | ------------------------------------------------------------------------------------------------- | ---------- |
| Quebra de contrato      | remover/renomear endpoint, campo ou arquivo; tornar um campo obrigatório; mudar tipo ou semântica | **MAJOR**  |
| Adição compatível       | novo endpoint, novo campo opcional, novo valor aceito                                             | **MINOR**  |
| Sem impacto no contrato | correção de texto, descrição, exemplo, erro de digitação                                          | **PATCH**  |

O campo `info.version` de cada spec acompanha a tag: ao publicar uma nova versão, atualize `info.version` dos arquivos alterados para o número da nova tag.

## Como consumir

Sempre fixe uma **tag**, nunca `main`: `main` pode conter mudanças ainda não publicadas.

**URL raw (leitura direta, ex.: Swagger UI/Redoc):**

```
https://raw.githubusercontent.com/dp-icea/openapi/v1.0.0/dss.yaml
```

```bash
git submodule add https://github.com/dp-icea/openapi.git openapi
cd openapi && git checkout v1.0.0 && cd -
git add .gitmodules openapi
git commit -m "chore: openapi v1.0.0"
```

Para atualizar: `cd openapi && git fetch --tags && git checkout vX.Y.Z`, e faça commit do novo ponteiro do submódulo.

## Como alterar uma spec

1. Crie um branch a partir de `main` e edite os arquivos aqui (não nos repositórios consumidores).
2. Valide:
   ```bash
   npx @redocly/cli@1.34.0 lint --extends=minimal *.yaml
   ```
3. Abra um PR. Após o merge, publique a versão:
   - atualize `info.version` dos arquivos alterados;
   - adicione a entrada no [CHANGELOG](CHANGELOG.md);
   - crie e envie a tag:
     ```bash
     git tag -a vX.Y.Z -m "vX.Y.Z"
     git push origin vX.Y.Z
     ```
4. Atualize a referência nos repositórios consumidores para a nova tag.
