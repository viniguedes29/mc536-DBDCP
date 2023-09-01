# Equipe Dinossauros, Bancos de Dados e Coisas Parecida (DBDCP)
## Membros
* Márcio Levi Sales Prado - 183680
* Francisco Vinicius Sousa Guedes - 260440
* Davi Gabriel Bandeira Coutinho - 183710

### Modelo conceitual ER revisado
![](images/diagramaER_revisado.png)
*Diagrama ER revisado*
### Mapeamento para o modelo relacional
NUTRIENTE(<u>nome</u>, tipo)
POSSUI(<u>nome_ingrediente, nome_nutriente</u>, porcentagem)
    nome_ingrediente chave estrangeira -> INGREDIENTE(nome) <br>
    nome_nutriente chave estrangeira -> NUTRIENTE(nome)<br>
INGREDIENTE(<u>nome</u>, tipo)<br>
CONTÉM(<u>nome_ingrediente, nome_porcao</u>, quantidade, tipo_porcao)<br>
    nome_ingrediente chave estrangeira -> INGREDIENTE(nome) <br>
    nome_porcao chave estrangeira -> PORCAO_CARDAPIO(nome)<br>
SOBREMESA(<u>nome, data</u>)<br>
ESCOLHE_SOBREMESA(nome_sobremesa, data, RA)<br>
    RA chave estrangeira -> ESTUDANTE(RA)<br>
    nome_sobremesa chave estrangeira -> SOBREMESA(nome)<br>
PRATO_PRINCIPAL(<u>nome, data</u>)<br>
ESCOLHE_PRATO_PRINCIPAL(nome_prato, data, RA)<br>
    RA chave estrangeira -> ESTUDANTE(RA)<br>
    nome_prato chave estrangeira -> PRATO_PRINCIPAL(nome)<br>
GUARNIÇÃO(<u>nome, data</u>)<br>
ESCOLHE_GUARNIÇÃO(nome_guarnição, data, RA)<br>
    RA chave estrangeira -> ESTUDANTE(RA)<br>
    nome_guarnição chave estrangeira -> GUARNIÇÃO(nome)<br>
ESTUDANTE(nome, <u>RA</u>)<br>
CARDÁPIO(<u>data, turno</u>, nome_prato_principal, nome_sobremesa, nome_guarnição)
    