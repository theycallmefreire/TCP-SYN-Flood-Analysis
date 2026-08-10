# Análise de Incidente — TCP SYN Flood

## Visão Geral

Neste laboratório, foi realizada uma análise técnica de segurança de redes a partir de um cenário simulado de indisponibilidade de um servidor web.

O incidente foi identificado após um alerta de monitoramento indicar que o serviço apresentava falhas de conexão. A partir disso, foi realizada uma análise do tráfego de rede utilizando um sniffer de pacotes, com o objetivo de identificar a origem e o comportamento do tráfego anômalo.

Durante a investigação, foi identificado um volume elevado de requisições TCP `SYN` provenientes de um único endereço IP e direcionadas ao servidor. O padrão observado é compatível com um ataque de **TCP SYN Flood**, uma técnica de **Denial of Service (DoS)** que explora o processo de estabelecimento de conexões TCP.

A técnica consiste no envio de múltiplas solicitações `SYN` sem a conclusão adequada do processo de estabelecimento da conexão. Como consequência, o servidor pode manter um número elevado de conexões parcialmente estabelecidas, consumindo recursos e potencialmente impedindo o atendimento de conexões legítimas.

## Análise do Tráfego

A captura de pacotes utilizada durante a investigação apresentou o padrão de tráfego que fundamentou a identificação do incidente.

<img width="1172" height="643" alt="1767831707916" src="https://github.com/user-attachments/assets/fbc7c536-7632-44fc-8d58-aabccc234aac" />


O principal indicador observado foi a concentração de requisições `SYN` provenientes da mesma origem, associada ao comportamento anômalo de estabelecimento de conexões.

Esse padrão, em conjunto com a indisponibilidade observada no serviço, forneceu evidências para caracterizar o evento como um possível **TCP SYN Flood**.

## Resposta ao Incidente

Como medida inicial de contenção, foram adotadas as seguintes ações:

* Isolamento temporário do servidor afetado, permitindo a recuperação dos recursos e reduzindo o impacto do tráfego malicioso.
* Implementação de uma regra de firewall para bloquear o endereço IP identificado como origem do tráfego.

O bloqueio do endereço IP foi eficaz como medida de contenção no cenário analisado. Entretanto, essa abordagem possui limitações quando utilizada isoladamente, principalmente em cenários envolvendo **IP spoofing** ou ataques distribuídos (**DDoS**).

## Mitigação

A análise também evidencia a necessidade de mecanismos de proteção mais robustos para ambientes de produção. Entre as medidas aplicáveis estão:

* **SYN Cookies**, para reduzir o consumo de recursos associado a conexões TCP parcialmente estabelecidas;
* **Rate Limiting**, para controlar a taxa de novas conexões;
* **NGFW**, com recursos adicionais de inspeção e filtragem de tráfego;
* Soluções específicas de **mitigação de DDoS**, especialmente em cenários distribuídos.

## Conclusão

O laboratório permitiu aplicar, de forma prática, conceitos relacionados à análise de tráfego de rede, identificação de anomalias e resposta a incidentes de segurança.

A investigação demonstrou a importância da análise de pacotes para identificar padrões anômalos e auxiliar na determinação da natureza de um incidente. Também evidenciou que a resposta rápida é fundamental para reduzir o impacto sobre a disponibilidade dos serviços.

Este laboratório faz parte do processo de desenvolvimento prático em **Network Security, Traffic Analysis e Incident Response**, com foco na identificação e mitigação de ameaças à disponibilidade de serviços.
