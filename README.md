# mod_sec
Regras_Criadas_SEPE2023

Um WAF, ou "Web Application Firewall" (Firewall de Aplicação Web), é uma medida de segurança cibernética que protege um site ou aplicação web contra ataques maliciosos. Funciona de maneira simples da seguinte forma:

1. **Monitoramento do Tráfego**: O WAF fica entre os usuários da web e o servidor da aplicação, interceptando todo o tráfego da web que entra e sai. 

2. **Filtragem de Tráfego**: Ele analisa o tráfego em busca de atividades suspeitas, como tentativas de invasão, exploração de vulnerabilidades e ataques de injeção de código.

3. **Regras de Segurança**: O WAF aplica um conjunto de regras de segurança personalizadas ou pré-configuradas. Essas regras determinam o que é permitido e o que é bloqueado. Por exemplo, ele pode bloquear tráfego que pareça tentar explorar vulnerabilidades conhecidas.

4. **Bloqueio de Ataques**: Quando o WAF detecta tráfego malicioso que viola as regras de segurança, ele bloqueia esse tráfego, impedindo que os ataques atinjam a aplicação web. Pode também gerar alertas para que os administradores tomem medidas apropriadas.

5. **Proteção em Tempo Real**: O WAF age em tempo real, o que significa que ele verifica cada solicitação e resposta à medida que acontecem, protegendo a aplicação web contra ameaças instantaneamente.

Em resumo, um WAF funciona como um guarda de trânsito para o tráfego da web, identificando e bloqueando qualquer atividade suspeita que possa representar uma ameaça à segurança da aplicação web. Ele ajuda a proteger sites e aplicativos contra ataques cibernéticos, mantendo-os seguros e funcionando corretamente.

Um Web Application Firewall (WAF) opera principalmente na camada de aplicação da arquitetura de rede. A explicação anterior se concentra principalmente nessa camada, pois é onde o WAF desempenha seu papel mais significativo. Aqui está uma explanação mais detalhada de como ele opera em diferentes camadas:

1. **Camada de Aplicação**: O WAF é projetado para proteger as aplicações web, o que ocorre na camada de aplicação. Ele monitora o tráfego HTTP/HTTPS, analisando as solicitações e respostas da aplicação para identificar possíveis ameaças, como injeção de SQL, ataques de Cross-Site Scripting (XSS), entre outros.

2. **Camada de Transporte**: Embora o WAF se concentre principalmente na camada de aplicação, ele também pode verificar informações em camadas inferiores. Isso pode incluir a inspeção de cabeçalhos HTTP (que estão na camada de transporte) para detectar padrões suspeitos ou tentativas de ataques.

3. **Camada de Rede**: Embora o WAF não seja estritamente uma solução de camada de rede, alguns WAFs avançados podem incluir funcionalidades de filtragem de tráfego na camada de rede para bloquear tráfego indesejado ou malicioso antes que ele chegue à camada de aplicação. Isso é menos comum e não é a função principal de um WAF, mas é uma extensão possível.

Portanto, a explicação anterior enfatiza a camada de aplicação, pois é onde o WAF desempenha o papel mais crítico na proteção das aplicações web contra ameaças. Ele se concentra em garantir que as solicitações e respostas da aplicação estejam seguras e em conformidade com as regras de segurança, identificando atividades maliciosas nesse nível.

O ModSecurity é um firewall de aplicação web (WAF) de código aberto que atua como um escudo de segurança para aplicações web, protegendo-as contra ameaças cibernéticas, como injeções de SQL, ataques de Cross-Site Scripting (XSS) e muitos outros. Ele é amplamente usado para aumentar a segurança de servidores web e aplicativos.

Vou explicar o ModSecurity, seu modo de configuração padrão e como adicionar o Core Rule Set, além de fornecer um exemplo de arquivo de configuração com o modo "Paranoia Level 2":

**Modo de Configuração Padrão do ModSecurity**:
- O ModSecurity pode ser configurado em diferentes modos, mas o modo de configuração padrão envolve a detecção de ataques e a geração de registros (logs) para análise posterior.
- No modo padrão, o ModSecurity monitora as solicitações HTTP, verifica-as em busca de padrões de ataques conhecidos e registra eventos suspeitos.
- Ele não bloqueia automaticamente as solicitações, mas pode gerar logs detalhados para revisão.

**Adição do Core Rule Set (CRS) e Benefícios**:
- O Core Rule Set é um conjunto de regras de segurança pré-configuradas e mantidas pela comunidade de segurança cibernética. Ele é projetado para ajudar a proteger contra uma ampla gama de ameaças cibernéticas comuns.
- Adicionar o Core Rule Set ao ModSecurity é altamente recomendado, pois fornece um conjunto abrangente de regras de segurança prontas para uso.
- Os benefícios incluem uma proteção mais robusta contra ataques web comuns, economia de tempo, uma configuração mais segura e a capacidade de bloquear automaticamente solicitações maliciosas.
- O CRS é altamente personalizável, permitindo ajustar o nível de segurança com base nas necessidades da aplicação.

**Exemplo de Arquivo de Configuração com o Modo "Paranoia Level 2"**:
- Para ativar o ModSecurity com o Core Rule Set e o modo "Paranoia Level 2", você precisará de um arquivo de configuração. Aqui está um exemplo simplificado:

```apache
SecRuleEngine On
SecDefaultAction "phase:2,log,deny"
SecRule REQUEST_COOKIES|!REQUEST_COOKIES:/__utm/|!REQUEST_COOKIES:/_pk/|!REQUEST_COOKIES:/_ga/|!REQUEST_COOKIES:/_pk_ref/|!REQUEST_COOKIES:/_pk_ses/|REQUEST_COOKIES_NAMES|ARGS_NAMES|ARGS|XML:/*|!REQUEST_HEADERS:Referer|!REQUEST_HEADERS:User-Agent "^(\w+/){2,3}$" "t:none,t:urlDecodeUni,t:normalizePath,block,id:'1000',

msg:'Potentially malicious input detected'"
SecRule REQUEST_HEADERS:User-Agent "(?:\b(?:msnbot|Googlebot|Bingbot|Slurp|DuckDuckBot|Baiduspider|YandexBot|Sogou|Exabot|Yahoo)\b)" "t:none,id:'1001',phase:2,log,pass,msg:'Search engine bot detected'"
```

Neste exemplo, o `SecRuleEngine` é ativado e definido para bloquear solicitações suspeitas. O "Paranoia Level 2" é alcançado através das regras definidas com `SecDefaultAction`, que bloqueiam e registram solicitações suspeitas.

Este é um exemplo simplificado e os arquivos de configuração podem variar dependendo da sua aplicação e requisitos específicos de segurança. Certifique-se de adaptar as regras de segurança e as configurações ao seu ambiente específico.
