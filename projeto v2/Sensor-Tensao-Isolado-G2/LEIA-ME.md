# Sensor de tensão isolado ±250 V, G2

Roteamento, desenho da placa e entrega final.
Chrysthofer Arthur, Felipe Augusto, João Pedro.

## Bloco V-SENS da placa-mãe, 29/10/2026

**Origem.** Na placa-mãe da turma, cada placa entra como um símbolo de 12 pinos da
biblioteca comum CEL111-2026-03, do repositório da disciplina
(https://github.com/pedroalmeida-ufjf/CEL111, commit `eb84493`, de 27/10/2026). O símbolo
do G2 é o V-SENS. Esta pasta traz o footprint dele e a biblioteca de símbolos com o
footprint atribuído; a placa e o esquemático não mudaram.

**Arquivos novos.**

- `CEL111-2026-03.pretty/V-SENS.kicad_mod`: o footprint, no molde do `I-SENS.kicad_mod` da
  mesma biblioteca. É a placa vista de cima, com a origem no meio da borda de cima, o
  contorno de 52,0 mm por 36,5 mm na serigrafia e uma ilha sobre cada pino de J2, J3 e J1,
  numerada como o pino do símbolo.
- `CEL111-2026-03.kicad_sym`: a biblioteca de símbolos do repositório da disciplina com uma
  única mudança, o campo Footprint do V-SENS, que passa a `CEL111-2026-03:V-SENS`. O I-SENS
  e o RELAY ficam como estão.
- `fp-lib-table` e a linha nova da `sym-lib-table`: as duas bibliotecas, por caminho
  relativo ao projeto, `${KIPRJMOD}`.
- `Sensor-Tensao-Isolado.step`: o modelo 3D da placa, com a origem no canto inferior
  esquerdo, os componentes, o cobre, a máscara e a serigrafia. Sai de uma cópia temporária
  da placa que recebe os modelos da biblioteca padrão do KiCad 10, como as vistas 3D.

**Ilhas.** Ovais de 1,6 mm por 2,5 mm, como as do I-SENS, com o eixo maior perpendicular à
fileira. O furo é de 1,0 mm, o mesmo da barra de pinos da placa; o I-SENS usa 0,8 mm. As
posições são as dos pinos na placa, sem arredondamento: o J3 fica 0,08 mm à direita do J2,
como na placa. Seis cotas, em Dwgs.User, dão a posição de cada conector em relação ao
contorno. Coordenadas em relação à origem, com y para baixo:

| Ilha | Pino da placa | Rede | x (mm) | y (mm) |
|---|---|---|---|---|
| 1 | J2.1 | VIN+ | -22,12 | 2,50 |
| 2 | J2.2 | VIN+ | -19,58 | 2,50 |
| 3 | J2.3 | VIN+ | -17,04 | 2,50 |
| 4 | J2.4 | VIN+ | -14,50 | 2,50 |
| 5 | J3.1 | VIN- | -22,04 | 34,50 |
| 6 | J3.2 | VIN- | -19,50 | 34,50 |
| 7 | J3.3 | VIN- | -16,96 | 34,50 |
| 8 | J3.4 | VIN- | -14,42 | 34,50 |
| 9 | J1.1 | AMC_OUT | 15,50 | 10,46 |
| 10 | J1.2 | +5V | 15,50 | 13,00 |
| 11 | J1.3 | GND | 15,50 | 15,54 |
| 12 | J1.4 | GND | 15,50 | 18,08 |

**Modelo 3D no footprint.** O V-SENS aponta para
`${KIPRJMOD}/../sensor tensao/Sensor-Tensao-Isolado.step`, com deslocamento de
(-26; -36,5; 9) mm, como o (-10; -18; 9) do I-SENS. Na árvore do repositório da disciplina,
o projeto da placa-mãe fica em `2026-03/mcu temp/` e esta placa em `2026-03/sensor tensao/`,
e o caminho leva a `2026-03/sensor tensao/Sensor-Tensao-Isolado.step` em qualquer cópia do
repositório. Neste projeto o caminho não existe, e o V-SENS fica sem modelo.

**Correspondência com o repositório da disciplina.** O `V-SENS.kicad_mod` corresponde a
`2026-03/CEL111-2026-03.pretty/`, ao lado do `I-SENS.kicad_mod`; a `CEL111-2026-03.kicad_sym`
difere da `2026-03/CEL111-2026-03.kicad_sym` só no campo Footprint do V-SENS; e o STEP
corresponde a `2026-03/sensor tensao/`.

**Conferência.** No KiCad 10.0.7: com a origem posta em (191; 60) mm da placa, as 12 ilhas
do V-SENS coincidem com os pinos de J2, J3 e J1 da tabela acima; o V-SENS sozinho numa
placa de teste dá 0 violações no DRC; numa placa-mãe de teste, na árvore do repositório da
disciplina, a vista 3D mostra os pinos de J2, J3 e J1 centrados sobre as ilhas, uns 0,5 mm
acima da placa-mãe; e o ERC e o DRC
deste projeto dão o mesmo resultado da rev. E.

## Rev. E, 25/10/2026

**Um MCP601 só.** Por indicação do professor, saíram o U3, seguidor da referência, o
divisor R11 e R12 e o C15, que filtrava o nó desse divisor. A VREF vem agora de um divisor
ligado direto à entrada + do U2: R8, de 21,5 kΩ, para o +3.3VA, e R13, de 13,0 kΩ, para o
GND. O equivalente é 8,101 kΩ, 0,51 % acima do R10, de 8,06 kΩ, e a referência fica em
1,2435 V. O C11 passou a ir do nó ao GND, com polo em 16,4 kHz. O C14, que desacoplava o
U3, ficou no +3.3VA, e no esquemático foi para junto do C13.

**Placa.** O R13 ocupa o lugar do C15, girado 180 graus, e o C11 também girou 180 graus,
para o pad do nó ficar sob o do R13. Saíram as trilhas do VREF, do nó do divisor antigo e os
ramos de GND que só serviam ao R12 e ao C15; o +3.3VA que ia ao R11 desce até o pad 1 do R8.
O U1 não mudou, e no lado de alta só entraram as vias de costura de HGND.

**Vias de costura de HGND.** Os 11 pads de HGND são de montagem em superfície, na face de
cima, e a placa não tinha via de HGND: o preenchimento de HGND da face de baixo, de 396,8 mm²,
não se ligava a nada. Quatro vias de 0,6/0,3 mm, o tamanho da netclass, ligam a zona de HGND
das duas faces, em (171,00; 79,50), (166,25; 64,50), (179,00; 92,25) e (180,25; 82,50) mm.
Cada uma fica inteira dentro dos dois preenchimentos, com 0,3 mm de cobre em volta, e longe
de serigrafia e de corpo de peça.

**C17.** No esquemático, a especificação do C17 passou a X5R, 16 V, 1206, cerâmico, como na
nota 8, na lista de materiais e no relatório.

**Serigrafia refeita.** Todas as referências em 0,8 mm, ao lado da sua peça, sem tocar ilha,
corpo de outra peça ou outro texto; os textos de J1, J2 e J3 espelhados na face de
baixo; os rótulos da placa dentro do contorno.

**Conferência.** ERC: 0 erros e 0 avisos. DRC com zonas repreenchidas e paridade: 0 erros,
0 ligações por rotear, nenhum aviso de serigrafia e nenhum cobre isolado; a paridade acusa
só o campo Especificação ausente nos 32 footprints.

**Vistas 3D.** A placa não traz modelos 3D nos footprints. As vistas da pasta `figuras/`
saem de uma cópia temporária da placa que recebe os modelos da biblioteca padrão do KiCad
10 para cada footprint.

## O que mudou

Esta seção e as duas seguintes vêm da entrega do roteamento, de 19/10/2026, na rev. D: o
formato KiCad 7 e o DRC que não pôde rodar se referem àquela entrega. Os arquivos de hoje
estão no formato do KiCad 10, e o DRC roda, com o resultado das seções acima.

**Formato dos arquivos.** A placa está gravada em formato KiCad 7. O KiCad 9 ou 10
abre sem problema e oferece atualizar ao salvar. O esquemático continua no formato
original (KiCad 10), alterado só na nota 8.

**Barreira de isolação: 3,0 mm → 8,0 mm.** As zonas estavam em x = 183,5 e 186,5.
Passaram para 181,0 e 189,0. O AMC3330 DWE especifica creepage e clearance de no
mínimo 8 mm, e a seção 5.6 do SBASA34B manda o projeto da placa não reduzir isso.

**Contorno: 45 × 36,5 mm → 52 × 36,5 mm.** Alarguei em x de 165..210 para 165..217
para o lado de baixa não perder área com a barreira maior.

**Catorze componentes posicionados.** R6 a R12 e C11 a C17 estavam fora do contorno.
Os catorze são do lado de baixa. C16 e C17 junto do AMS1117, R6 e C13 e C14 na faixa
central, as redes do U2 e do U3 em duas colunas à direita (x = 206,8 e 212,8).

**Roteamento completo.** 326 segmentos e 31 vias novos. GND e HGND foram roteados com
trilha, não só pelo preenchimento, porque as trilhas novas espremiam o cobre e alguns
pads ficavam sem contato com a zona.

**Regras elétricas.** Trilha 0,25 mm (0,4 mm nas alimentações), clearance 0,25 mm,
via 0,6/0,3 mm. A zona HGND recua 1,5 mm das trilhas do lado de alta, porque HGND fica
no meio da tensão medida e vê cerca de 125 V em relação a cada extremo do divisor.
O arquivo de projeto aplica a classe HV a VIN+, VIN- e aos nós entre R1 e R2 e entre R4
e R5; HGND e INP ficam na classe padrão, com a mesma via de 0,6/0,3 mm.

**Serigrafia.** Identificação do grupo, marcação dos dois lados e linhas delimitando a
barreira de 8 mm.

**Nota 8 do esquemático.** Passou a dizer que o C17 é cerâmico, como o professor pediu,
e registra que o datasheet do AMS1117 pede tântalo sólido de 22 µF porque a malha usa a
ESR dele na compensação. Se aparecer ondulação de alta frequência na saída do regulador,
a correção é 0,5 a 1 Ω em série com o C17.

## Medições feitas no arquivo entregue

| Item | Valor |
|---|---|
| Ligações em aberto | 0 |
| Menor clearance entre redes | 0,255 mm |
| Cobre do lado de alta até cobre do lado de baixa | 7,25 mm |
| VIN+ até VIN- | 23,8 mm |
| VIN+ (ou VIN-) até HGND | 1,50 mm |
| VIN+ até o nó R1/R2 | 1,83 mm |
| Cobre até a borda | 0,55 mm |

Os 7,25 mm entre os dois lados são o limite do próprio footprint do U1: as ilhas do
SOIC-16W já reduzem os 8 mm do encapsulamento. Para passar disso seria preciso fresar
um rasgo na barreira, sob o corpo do CI, como a seção 5.6 do datasheet sugere.

## Confira ao abrir

1. Pressione B para repreencher as zonas.
2. Rode o DRC. Eu não pude rodar o DRC do KiCad aqui: a versão que meu ambiente
   instala é a 7.0.11, e o `kicad-cli` dela não tem o comando `drc`. As medições acima
   vieram de verificação geométrica própria sobre o arquivo final, mais a conectividade
   do módulo Python do pcbnew.
3. Confira a classe HV no diálogo de regras de projeto.
4. O footprint do U1 aponta para o SOIC-16W nominal. Antes de fabricar, confira contra
   o desenho mecânico do DWE, que é a variante de alta isolação.
