# Desafio DIO 2 

**1. Criar uma instância na Azure para MySQL:** 

    Criado no azure com sucesso.

**2. Criar o Banco de dados com base disponível no github:**

    Tive alguns problemas na inserção de dados, onde aparecia varios erros:

```
"Cannot add or update a child row: a foreign key constraint fails (azure_company.employee, CONSTRAINT fk_employee FOREIGN KEY (Super_ssn) REFERENCES employee (Ssn) ON DELETE SET NULL ON UPDATE CASCADE)"
"Cannot add or update a child row: a foreign key constraint fails (azure_company.dependent, CONSTRAINT fk_dependent FOREIGN KEY (Essn) REFERENCES employee (Ssn))"
"Cannot add or update a child row: a foreign key constraint fails (azure_company.departament, CONSTRAINT departament_ibfk_1 FOREIGN KEY (Mgr_ssn) REFERENCES employee (Ssn))"
"Cannot add or update a child row: a foreign key constraint fails (azure_company.dept_locations, CONSTRAINT fk_dept_locations FOREIGN KEY (Dnumber) REFERENCES departament (Dnumber))"
"Cannot add or update a child row: a foreign key constraint fails (azure_company.project, CONSTRAINT fk_project FOREIGN KEY (Dnum) REFERENCES departament (Dnumber))"
"Cannot add or update a child row: a foreign key constraint fails (azure_company.works_on, CONSTRAINT fk_employee_works_on FOREIGN KEY (Essn) REFERENCES employee (Ssn))"]
 ```
    Consultando as issues achei uma [solução](https://github.com/julianazanelatto/power_bi_analyst/issues/4)
    [GitHub](https://github.com)
    
**3. Integrar Power BI com MySQL no Azure:**

    As configurações foram ajustadas utilizando o arquivo do Azure denominado "desafio-projeto-dio-rafa_azure_company.pbids"
 
**4. Verificar problemas na base a fim de realizar a transformação dos dados
Diretrizes para transformação dos dados:**

***4.1. Verificar os cabeçalhos e tipos de dados***
***4.2. Modificar os valores monetários para o tipo double preciso***

    Na tabela 'employee', o tipo de dados da coluna 'Salary' foi atualizado de Decimal para Decimal Fixo.

***4.3. Verificar a existência dos nulos e analise a remoção***

    O valor nulo na linha 5 da tabela 'employee' sugere que esse registro corresponde a um gerente.

***4.4. Os employees com nulos em Super_ssn podem ser os gerentes. Verificar se há algum colaborador sem gerente***

    Todos os colaboradores têm um gerente designado. Nenhum funcionário está sem supervisão, pois cada um está vinculado a um gerente específico na organização.

***4.5. Verificar se há algum departamento sem gerente***

    Cada departamento possui um gerente designado. Não há departamentos na organização sem a supervisão de um gerente.

***4.6. Se houver departamento sem gerente, suponha que você possui os dados e preencha as lacunas***

    Não houve necessidade

***4.7. Verifique o número de horas dos projetos***

    275 horas em todos os 6 projetos

***4.8. Separar colunas complexas***

    Os dados da coluna 'Address' na tabela 'employee' foram desmembrados em quatro novas colunas: 'AddressNumber', 'Street', 'City' e 'State', utilizando um delimitador '-' para separar os elementos.

***4.9. Mesclar consultas employee e departament para criar uma tabela employee com o nome dos departamentos associados aos colaboradores. A mescla terá como base a tabela employee. Fique atento, essa informação influencia no tipo de junção***

    Usando 'Dno' da tabela 'employee' com 'Dnumber' da tabela 'department' o resultado foi renomedado como 'employee_department'

***4.10. Neste processo elimine as colunas desnecessárias.***

    'Dno', 'azure_company.departament', 'azure_company.dependent', 'azure_company.works.on', 'azure_company.dept_locations', 'azure_company.employee' e 'azure_company.project' foram removidas.

***4.11. Realize a junção dos colaboradores e respectivos nomes dos gerentes. Isso pode ser feito com consulta SQL ou pela mescla de tabelas com Power BI. Caso utilize SQL, especifique no README a query utilizada no processo.***

    Houve uma mesclagem das tabelas 'employee' com base nas colunas 'Super_ssn' e 'Ssn', dando origem à tabela 'employee_manager'.
    Enquanto isso, ocorreu uma combinação das colunas 'azure_company employee.Fname', 'azure_company employee.Minit' e 'azure_company employee.Lname' utilizando um separador de espaço, resultando na nova coluna 'Mname', que identifica o nome do gerente associado.

***4.12. Mescle as colunas de Nome e Sobrenome para ter apenas uma coluna definindo os nomes dos colaboradores***

    As colunas 'azure_company employee.Fname', 'azure_company employee.Minit' e 'azure_company employee.Lname' foram combinadas usando um separador de espaço para criar a nova coluna 'Ename' na tabela 'employee_manager'.
    Além disso, foram excluídas as colunas 'azure_company.departamento', 'azure_company.dependente' e 'azure_company.trabalha_em' da mesma tabela.
    Por fim, as colunas 'Dlocation' e 'Dname' foram reorganizadas e combinadas sob o nome 'Store'.

***4.13. Mescle os nomes de departamentos e localização. Isso fará que cada combinação departamento-local seja único. Isso irá auxiliar na criação do modelo estrela em um módulo futuro.***

    As tabelas 'departament' e 'dept_locations' foram combinadas com base na correspondência das colunas 'Dnumber', resultando na criação de uma nova tabela denominada 'departament_dlocations'.
    Durante esse processo, as colunas 'azure_company.dept_locations', 'azure_company.employee', 'azure_company.project', 'azure_company.departamento' e 'Dnumber.1' foram eliminadas da nova tabela resultante.

***4.14. Explique por que, neste caso supracitado, podemos apenas utilizar o mesclar e não o atribuir.***

    Usamos mesclar quando queremos juntar informações de diferentes tabelas e é importante manter detalhes específicos de cada uma delas.
    
***4.15. Agrupe os dados a fim de saber quantos colaboradores existem por gerente***

    Criado uma nova tabela apartir da 'employee_manager' e renomeada para 'manager_employee' e a contagem foi realizada na coluna 'Qty_employees' 

***4.16. Elimine as colunas desnecessárias, que não serão usadas no relatório, de cada tabela***
