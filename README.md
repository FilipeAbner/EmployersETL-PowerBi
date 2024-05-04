# Introdução

Este projeto foi concebido com o propósito de aplicar os conceitos fundamentais do processo ETL (Extração, Transformação e Carregamento) e criar um relatório para monitorar a precisão e eficácia desse processo. Para alcançar esse objetivo, configuramos uma instância de banco de dados MySQL hospedada na plataforma Azure, onde os dados serão armazenados. Utilizamos o Power BI para criar o relatório e o MySQL Workbench para gerenciar e realizar consultas SQL.

Ao longo deste trabalho, exploramos técnicas de extração de dados de diversas fontes, transformação para adequar os dados ao nosso propósito analítico e carregamento dos dados em um formato adequado para a criação de insights por meio do Power BI.

A combinação dessas ferramentas e técnicas nos permite realizar uma análise abrangente dos dados, identificar tendências, anomalias e padrões que são cruciais para tomadas de decisão informadas no contexto de ciência de dados.

# ETL

Abaixo será listado decisões tomadas durante o processo de ETL:

* Renomeação de colunas para melhor entendimento(ex: Dno -> Dnumber)
* Junção entre as tabelas de Empregado e Departamento
* Renomeaçao de diversas colunas como departament.Dname -> DepartmentName, departament.Dept_create_date -> Departament_create_date
* Juncao entre a tabela de empregado para encontrar quais os gerentes de cada empregado. 
* Mescla dos nomes dos empregados(first,mid,last) e de seus respectivos gerentes(firts,mid,last).
* Combinação das tabelas de departamento e suas respectivas localizações, tornando-as unicas.
* Consulta SQL para identificar os colaboradores de cada funcionario(quais funcionario trabalham no mesmo projeto).
    ```SQL
    SELECT DISTINCT
    CONCAT(e1.Fname, ' ', e1.Minit, ' ', e1.Lname) AS employee_name,
        CONCAT(e2.Fname, ' ', e2.Minit, ' ', e2.Lname) AS  collaborators
    FROM 
        azure_company.employee e1
    INNER JOIN 
        azure_company.works_on w1 ON e1.Ssn = w1.Essn
    INNER JOIN 
        azure_company.works_on w2 ON w1.Pno = w2.Pno
    INNER JOIN 
        azure_company.employee e2 ON w2.Essn = e2.Ssn AND e1.Ssn != e2.Ssn
    ORDER BY employee_name DESC;
    ```
* Juncao entre as tabelas Department e Employer para identificar quais os gerentes de cada Departamento
