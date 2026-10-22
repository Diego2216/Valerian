# VALERIAN — Roadmap mestre e piloto Philips Ambilight
Atualizado em 22/10/2026. Status: planejamento técnico; não confundir com recursos implementados.

## Estado confirmado do MVP
- PWA hospedada em https://diego2216.github.io/Valerian/
- Reconhecimento de fala e respostas por voz no iPhone, com ativação manual e limitações do Safari/iOS.
- Controles de quatro luzes virtuais (sala, quarto, cozinha, escritório), rotinas demonstrativas, player virtual, navegação e comandos de mídia/volume simulados.
- Ainda **não** há integração comprovada com dispositivos físicos, Spotify, IA generativa ou reconhecimento de voz permanente em segundo plano.
- Preservar funcionamento atual e testar regressões antes de cada publicação.

## Princípios arquiteturais
1. Camada de intenção: normalização pt-BR, sinônimos, frases compostas, desambiguação, memória de contexto com expiração, retorno estruturado e histórico.
2. Motor de ações: adaptadores de dispositivos (virtual, Home Assistant, Matter, fabricantes), confirmação do estado observado, timeout, retry limitado, idempotência e logs.
3. Segurança: autenticação, perfis e permissões; confirmação adicional para fechaduras/portões; credenciais exclusivamente no backend/armazenamento seguro, nunca no GitHub Pages; consentimento para áudio, câmera e presença.
4. Operação: modo demo explícito, testes automatizados, indicadores de conexão, diagnóstico, logs locais, rollback e documentação.
5. Custos: priorizar recursos locais e serviços gratuitos, sem prometer disponibilidade ilimitada. Não exigir Windows ligado para o PWA; integrações LAN podem exigir hub/servidor ligado.

## Backlog completo consolidado
### P0 — inteligência de comandos
- Comandos múltiplos e sequenciais; interpretação contextual e memória curta de cômodo/ação.
- Sinônimos e variações de linguagem; esclarecimento de ambiguidades; confirmação verbal da ação executada.
- Estado visual ouvir/processar/executar/falar, com diagnóstico de erros de microfone, TTS e dispositivo.
- Rotinas criadas/editadas por voz, horários, condições e regras de conflitos; painel personalizável.
- Comandos para mídia: play/pause, faixa anterior/próxima, volume absoluto/relativo, mudo e navegação.
- Testes de regressão em Safari iOS e desktop; controle virtual preservado.

### P1 — experiência e personalização
- Cérebro híbrido local + IA conectada (opcional), personalidade e voz configuráveis, respostas conversacionais.
- Preferências e memória autorizada, perfis família, modo visitante, painel familiar, agenda/tarefas/lembretes.
- Esfera animada e interface responsiva; painel ambiental com clima e calendário mediante integrações.
- Atalhos do iPhone e modo offline parcial; notificações com opt-in.
- Mapa/gêmeo digital da casa, memória espacial de dispositivos e objetos cadastrados.
- Rotinas de presença, sono, cinema, chegada e saída.

### P2 — dispositivos e automações
- Home Assistant, Matter, Zigbee via hub, Wi-Fi/Bluetooth quando compatíveis e APIs oficiais.
- Luzes/fitas LED, tomadas, interruptores, TVs e streaming, ar-condicionado/controle IR, ventiladores, cortinas, robôs aspiradores, sensores de presença/porta/temperatura, câmeras/campainhas, fechaduras/portões, irrigação, medidores de energia e eletrodomésticos suportados.
- Iluminação adaptativa, monitor de consumo e custos, alertas de segurança, condições baseadas em sensores, automação preditiva opt-in.
- Segurança reforçada para abertura/destravamento, ações perigosas e gravação de imagens.

### P3 — expansão e produto
- Assistente proativo sob autorização; agentes especializados, automação de tarefas e visão computacional com consentimento.
- Gestos via câmera opcional; vários painéis sincronizados, múltiplas residências, VALERIAN Business.
- Marketplace de rotinas com revisão/permissões, planos comerciais, VALERIAN Hub físico dedicado.
- Integrações de música/rádio/podcasts e streaming sujeitas às APIs e licenças dos provedores.

## Piloto 01: TV Philips Ambilight (dispositivo físico)
**Estado: aguardando identificação do modelo e teste de conectividade. Não implementado nem validado.**
- Nem toda TV Philips Ambilight expõe o mesmo protocolo. Android TV/Google TV, Saphi, Titan OS e gerações distintas têm APIs diferentes; não assumir suporte a JointSpace, ADB, Chromecast, Ambilight API ou Home Assistant sem verificar o modelo.
- Obter: modelo exato da etiqueta ou Configurações > Sobre; ano aproximado; sistema da TV; presença de controle por rede/app; se TV e iPhone estão na mesma rede Wi-Fi.
- Pesquisa técnica depois da identificação: verificar integração Philips TV no Home Assistant, endpoints LAN locais se disponíveis, requisitos de pareamento/PIN, autenticação, CORS e suporte real a ligar via rede/Wake-on-LAN.
- **GitHub Pages não pode armazenar credenciais nem normalmente acessar APIs LAN da TV diretamente do navegador por CORS/mixed-content/restrições de rede.** Planejar gateway local seguro (Home Assistant em mini PC/Raspberry Pi/hub) ou ponte autorizada. Se for necessário hub sempre ligado, comunicar claramente.
- Escopo mínimo do teste real: descoberta/pareamento; consultar estado; ligar/desligar quando suportado; volume +/−, mudo; Ambilight ligar/desligar/cor/modo somente se API do modelo permitir; feedback de êxito real, não simulado.
- Comandos-alvo: “VALERIAN, ligar a TV da sala”; “desligar TV”; “aumentar o volume da TV”; “silenciar TV”; “ligar Ambilight”; “modo cinema”.
- Segurança: TV conectada à rede confiável, pareamento consentido, segredos fora do frontend, nenhuma porta LAN exposta publicamente.
- Critérios de aceite: ação física observada na TV, estado refletido no painel, mensagens de erro quando offline, regressão das funções virtuais e validação no iPhone.
- Plano alternativo se API nativa não existir: integração oficial de controle via Home Assistant ou emissor IR inteligente compatível (com limitações de leitura de estado e Ambilight).

## Etapas executáveis
1. Confirmar modelo Philips Ambilight e recursos de rede; escolher integração com evidências.
2. Implementar motor de comandos estruturados e adaptadores com testes em modo simulado.
3. Criar conector Philips adequado e pareamento seguro, sem chaves no PWA.
4. Testar comando real, estado e erros no iPhone; só então marcar integração como concluída.
5. Expandir gradualmente para outras categorias, preservando funcionamento existente.

## Evidência recebida: tela de software da TV (22/10/2026)
- Firmware exibido: TPM257L_V253.003.073.081; data da compilação 2026/05/28 11:48:39.
- TPM257L aparece associado a Philips Titan OS em relatos públicos, mas a plataforma/firmware **não identifica unicamente o modelo comercial**; solicitar modelo exato (ex. xxPUGxxxx/78) em Sobre a TV ou etiqueta traseira.
- Para Titan OS, integração via API JointSpace antiga não é garantida; relatos de controle via API local/MQTT são experimentais e dependentes de modelo. Não prometer controle Ambilight antes de validar.
- Próximo passo: confirmar modelo e TV/iPhone na mesma rede; depois testar descoberta e pareamento com método seguro apropriado.

## Modelo comercial confirmado em foto posterior
- Philips Ambilight **55PUG8100/78**, 55 polegadas, firmware **TPM257L_V253.003.073.081**. Não publicar número de série ou endereços MAC.
- Fonte oficial Philips: https://www.philips.com.br/c-p/55PUG8100_78/tv-led-4k-ambilight ; ficha https://www.documents.philips.com/assets/20251120/9924e7dc24fe497285fab39b009f15cd.pdf ; manual https://www.documents.philips.com/assets/20250626/bc9b71e9ab744d19a369b3080031fc18.pdf .
- **Confirmado pelo fabricante:** Titan OS, experiência Smart Home Matter, Apple AirPlay, Wi-Fi e Ethernet, Ambilight trilateral. A existência de Matter não garante acesso a todos os comandos desejados nem acesso direto pela PWA.
- Próxima verificação com usuário: iPhone e TV na mesma rede; localizar no menu TV integração Matter/Apple Home ou código de pareamento. Não solicitar senha Wi-Fi nem publicar PIN.
- Teste sugerido: parear TV no Apple Casa (se disponível) ou controlador Matter autorizado, validar ligar/desligar; em seguida avaliar ponte Home Assistant para VALERIAN e comandos adicionais. Ambilight e volume dependem das capacidades efetivamente expostas.

## Teste físico HomeKit confirmado (22/10/2026)
- TV Philips 55PUG8100/78 foi adicionada ao Apple Casa como TV via HomeKit/AirPlay; interface exibiu botão de energia e seleção de entradas.
- Usuário confirmou que ao acionar desligar no app Casa a **TV desligou fisicamente**. Controle OFF real comprovado, independente do VALERIAN.
- Teste de ligar novamente ainda pendente; não declarar ON confirmado.
- Matter no Apple Casa permaneceu marcado 'Incompatível', embora HomeKit tenha fornecido controle.
- Integração VALERIAN→TV **ainda não implementada**. PWA GitHub Pages não tem acesso direto às APIs HomeKit do app Casa; avaliar ponte local segura ou Atalhos iOS (conforme ações suportadas), sem presumir acesso remoto nem expor credenciais.

## Teste físico HomeKit bidirecional confirmado (22/10/2026)
- Após validar desligamento físico, usuário confirmou que a TV **voltou a ligar** pelo botão de energia do aplicativo Casa no iPhone.
- Resultado: HomeKit no iPhone controla ON e OFF reais da Philips 55PUG8100/78; ambos validados pelo usuário.
- Isto **não** significa integração já pronta no VALERIAN. Próxima etapa: avaliar bridge local autenticada ou fluxo de Atalhos do iOS que realmente exponha o controle, sem inventar API HomeKit acessível à PWA.
- Testes seguintes: controle de entrada, volume e Ambilight (sem assumir disponibilidade), falha de conexão, autenticação e retorno de estado.

## Teste ponta a ponta VALERIAN → Atalhos → HomeKit → Philips (22/10/2026)
- Usuário criou atalho iOS **VALERIAN Desligar TV** com ação Casa configurada para desligar a Philips 55PUG8100/78.
- Atalho testado diretamente no app Atalhos: TV desligou fisicamente.
- Botão **Desligar TV** do VALERIAN foi acionado no iPhone e **TV desligou fisicamente**, conforme confirmação explícita do usuário.
- Primeiro teste físico ponta a ponta de desligamento **APROVADO**. Não extrapolar para comando de voz, ligar, feedback de estado ou execução em segundo plano, ainda não testados pelo VALERIAN.
- Próximo teste: criar **VALERIAN Ligar TV** com ação Casa 'Ligar', validar execução direta no Atalhos e depois pelo botão VALERIAN; testar voz separadamente.

## Marco: ambos os botões VALERIAN controlam a TV (22/10/2026)
- Usuário confirmou: botões **Ligar TV** e **Desligar TV** no VALERIAN executam ações físicas reais na Philips 55PUG8100/78 via Atalhos iOS e HomeKit.
- Fluxo validado: VALERIAN PWA → deep link `shortcuts://run-shortcut` → Atalhos → HomeKit → TV.
- O iOS exibe confirmação 'Abrir com Atalhos?' antes da troca de aplicativo; **não há operação transparente em segundo plano** neste método.
- Próximos testes: voz para ligar/desligar TV; verificar se o iOS permite deep link iniciado por reconhecimento de fala (pode exigir gesto); testar entrada, volume e Ambilight apenas se compatíveis.

## Validação do desligamento por voz — 22/10/2026
- Usuário confirmou o fluxo real: pronunciou 'Desligar TV' no VALERIAN; iOS exibiu pop-up de autorização; após permitir, o app Casa/Atalhos abriu e a Philips desligou fisicamente.
- Portanto, o reconhecimento e encaminhamento de comando vocal para desligar a TV estão validados em primeiro plano, com intervenção explícita no pop-up iOS.
- Botões de ligar/desligar já validados; **ligar por voz ainda precisa de teste separado**.
- Não alegar operação hands-free, contínua ou sem confirmação do iOS.

## Pesquisa de integração sem pop-up / sem PC (22/10/2026)
- Requisito do usuário: falar **VALERIAN**, controlar Philips 55PUG8100/78 sem confirmação 'Abrir esta página no Atalhos?', usando somente iPhone + TV, sem PC.
- Philips confirma Titan OS e suporte Matter neste modelo: https://www.philips.com.br/c-p/55PUG8100_78/tv-led-4k-ambilight .
- Existem integrações comunitárias JointSpace para TVs Philips Titan OS, porém compatibilidade com **55PUG8100/78** e uso direto via Safari/PWA não foram demonstrados: https://github.com/daCream87/uc-philips-titanos .
- PWA hospedada em HTTPS não tem acesso direto à API HomeKit do iOS; `shortcuts://` abre outro app e gera confirmação de segurança do sistema, que não pode ser automaticamente aceita pelo JavaScript.
- Sem hub/controlador Matter ou servidor local e sem API TV confirmada compatível com Safari, não prometer controle sem pop-up.
- Manter integração atual funcionando; pesquisa futura: avaliar se a TV expõe API local e se navegador pode acessá-la de forma segura (HTTPS, CORS, autenticação, rede local); avaliar opções iOS nativas se necessário.
