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
