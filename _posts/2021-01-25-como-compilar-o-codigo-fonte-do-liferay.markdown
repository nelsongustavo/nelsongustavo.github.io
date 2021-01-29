---
layout: post
title:  "Como compilar o código fonte do Liferay"
date:   2021-01-25 20:30:00
category: Liferay
---

Na última quarta-feira o mundo tomou ciência de um novo Malware chamado freakout, que basicamente infecta servidores linux para servirem como cliente para ataques DDoS ou minerar criptomoedas.

Uma das vulnerabilidades foi identificada no Liferay, em todas as versões, porém a Liferay só disponibiliza patches de correção para distribuições enterprise.

Como temos vários portais em versões diferentes e até mesmo community, para aplicar o patch com as correções de segurança temos que re compilar o portal a partir do seu código fonte, isso pode até parecer uma tarefa fácil, mas não é.

A Liferay disponibiliza um [tutorial](https://portal.liferay.dev/participate/fix-a-bug/building-liferay-source) de como fazer isso, mas existem várias lacunas que não foram preenchidas, meu objetivo nesse post é preenchê-las.

## Pré requisitos

O Primeiro passo é garantir que você tenha instalado as seguintes ferramentas:

- [Apache Ant](https://ant.apache.org/manual/install.html)
- [Gradle](https://gradle.org/install/)
- [Liferay Blade CLI](https://learn.liferay.com/dxp/7.x/en/developing-applications/tooling/blade-cli/installing-and-updating-blade-cli.html)

## Clonando as dependências do portal

Clonar estas dependências do portal vai acelerar o processo de build.

```
git clone https://github.com/liferay/liferay-binaries-cache-2020 --branch master --single-branch --depth 1
```

## Clonando o portal

Vamos clonar o portal no mesmo diretório.

```
git clone git@github.com:liferay/liferay-portal.git
```

![Estrutura de diretórios](/assets/img/build.png)

> As dependências e o portal presiam estar dentro do mesmo diretório, observe a variável build.binaries.cache.dir no arquivo de configuração [build.properties](https://github.com/liferay/liferay-portal/blob/master/build.properties).

## Rode este comandos a partir do diretório do portal

```
ant all
```

> Esta task vai vai executar todas as outras, inclusive compilar e buildar o portal.

## Troubleshooting

Alguns problemas que encontrei ao fazer ete processo foram:

```
Please use Java 1.8.
```

Para resolver este problema instale A JDK 8 do Java e exporte a variável de ambiente JAVA_HOME apontando para a JDK no arquivo .bashrc ou .zshrc.


Outro problema que encontrei foi esse:

```
[typedef] Could not load definitions from resource org/apache/maven/artifact/ant/antlib.xml. It could not be found.
```

Adicionar o jar [maven-ant-tasks-2.1.3.jar](https://search.maven.org/artifact/org.apache.maven/maven-ant-tasks/2.1.3/jar) do antlib dentro da pasta liferay-portal/lib/development, resolveu o problema.

A lib do Jacoco também pode dar o mesmo problema.

```
  [taskdef] Could not load definitions from resource org/jacoco/ant/antlib.xml. It could not be found.
```

Se isso acontecer, basta adicionar o jar [jacoco-0.8.6.jar](https://mvnrepository.com/artifact/org.jacoco/org.jacoco.report/0.8.6) na pasta liferay-portal/lib/development

Também tive que setar um espaço de memória maior pro ant

```
export ANT_OPTS=-Xmx2560m
```

Bom, isso é tudo pessoal.



