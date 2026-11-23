---
name: ceramic-3d-architect
description: >-
  Projeta, reconstrói e modela objetos cerâmicos paramétricos em 2D/3D para o CeramiCAD Studio Pro a partir de texto, fotos de peças de argila prontas ou imagens de moldes de papel/gabaritos 2D existentes.
  Executa engenharia reversa multimodal, aplica os 5 princípios de manufaturabilidade cerâmica (continuidade de diâmetros, desenvolvibilidade em folhas A4, ângulos de chanfro/bisel, ancoragem com barbotina e tolerâncias de encaixe) e gera prompts de imagem estética para ateliê.
---

# Ceramic 3D Architect & Vision Engineer (CeramiCAD Studio Pro)

Esta habilidade atua como arquiteto cerâmico e engenheiro reverso computacional, transformando qualquer uma das 3 entradas abaixo em modelos 3D paramétricos e moldes 2D planificados para o CeramiCAD Studio Pro:

1. **Entrada Textual / Conceitual**: A partir do nome ou ideia de um objeto cerâmico (ex: *"Canoa parabólica com sulco guia"*, *"Bule inglês geométrico"*).
2. **Entrada Visual de Peça Pronta (Foto de Objeto Cerâmico / Argila)**: Engenharia reversa tridimensional (3D $\rightarrow$ 2D) a partir da foto de uma peça real, identificando proporções, espessura aparente, método construtivo e gerando os moldes desdobrados.
3. **Entrada Visual de Molde 2D / Gabarito Existente**: Engenharia reversa bidimensional (2D $\rightarrow$ 3D) a partir de scans, fotos de moldes de papel, cartolina ou diagramas técnicos, extraindo contornos de corte, linhas de vinco e montando a geometria 3D.

---

## 1. Protocolos de Entrada e Reconhecimento Multimodal

### Modo A: A partir de Nome ou Descrição Textual
- Decompõe a peça conceitualmente nos blocos funcionais do CeramiCAD.
- Define a linguagem estética, proporções ideais de bancada e acabamento.

### Modo B: A partir de Foto de Peça Cerâmica Pronta (3D $\rightarrow$ 2D)
Ao analisar a imagem de uma peça real de cerâmica/argila:
1. **Identificação do Método Construtivo**:
   - *Torno de Oleiro*: Simetria de revolução contínua, ranhuras circulares horizontais, marcas de corte de fio na base.
   - *Plaqueira (Slab Building)*: Presença de costuras de união com barbotina, paredes com espessura uniforme de placa, cantos chanfrados ou dobras vincadas.
   - *Pinch / Olaria Manual*: Superfície orgânica facetada, irregularidades táteis.
2. **Decomposição Morfológica**:
   - Localizar a linha d'água da base (pé), bojo, ombros, gargalo e boca.
   - Identificar acessórios (alças em tira/tubulares, bicos vertedores, pegas e tampas).
3. **Estimativa de Proporções & Escala Real**:
   - Determinar a relação Altura/Diâmetro ($H / \varnothing$).
   - Inferir a espessura de parede aparente ($E$, tipicamente entre $4.0\text{ mm}$ e $8.0\text{ mm}$ para utilitários).
4. **Geração do Plano de Desdobramento 2D**:
   - Planificar o corpo em placas A4 regradas (cilindros, cones, discos) ou polígonos vincados.

### Modo C: A partir de Imagem de Molde 2D / Gabarito de Papel (2D $\rightarrow$ 3D)
Ao analisar o desenho, foto ou scan de um molde plano existente:
1. **Classificação das Linhas Gráficas**:
   - *Linha Contínua*: Linha perimetral externa de **corte** da placa de argila (`outline`).
   - *Linha Tracejada / Pontilhada*: Linha de **vinco / dobra** (`creases`), determinando se a dobra é para dentro (*Valley Fold*) ou para fora (*Mountain Fold*).
   - *Hachura / Área Pontilhada*: Zona de escarificação e aplicação de **barbotina** (`anchors`).
2. **Extração Vetorial dos Vértices ($x, y$)**:
   - Mapear os pontos de contorno normalizados em milímetros reais em torno da origem central $(0,0)$.
3. **Cálculo da Reconstrução 3D**:
   - Simular o fechamento das dobras e a curvatura das placas para formar a peça sólida no espaço tridimensional.
   - Validar se as arestas que se encontram possuem comprimentos idênticos ($L_1 = L_2$) para garantir fechamento perfeito sem sobras.

---

## 2. As 5 Regras Matemáticas Inegociáveis de Manufatura Cerâmica

Independentemente do modo de entrada, o modelo resultante DEVE cumprir estritamente:

### 1. Continuidade de Diâmetro nas Juntas (Casamento Estanque)
Em blocos circulares/cônicos empilhados:
$$\varnothing_{\text{topo (Estágio } N)} = \varnothing_{\text{base (Estágio } N+1)}$$
Perímetro idêntico de união ($P = \pi \times \varnothing$) sem degraus estruturais acidentais.

### 2. Desenvolvibilidade 2D de Placas (*Slab Building*)
- **Superfícies Regradas (Desenvolvíveis 1:1 no A4)**:
  - *Cilindro*: retângulo plano ($P \times H$).
  - *Cone Truncado*: setor anular curvo (leque) com $R = \frac{r_{\text{maior}} \cdot s}{r_{\text{maior}} - r_{\text{menor}}}$, $r = R - s$, e $\theta = \frac{360^\circ \cdot r_{\text{maior}}}{R}$.
  - *Pirâmide Truncada / Assadeira Inclinada*: cada parede inclinada em ângulo $\alpha$ com a horizontal deve ser planificada como **trapézio isósceles real** (nunca retângulo), com largura inferior $L_{\text{base}}$, largura superior $L_{\text{topo}} = L_{\text{base}} + 2 \cdot \Delta y \cdot \cot(\alpha)$ e altura real de corte na placa (geratriz) $s = \frac{\Delta y}{\sin(\alpha)}$.
  - *Fundo / Disco*: círculo ou polígono plano.
- **Superfícies de Dupla Curvatura**: Bojos esféricos devem ser decompostos em cones chanfrados sucessivos (*facetamento*), gomos de pétalas verticais ou placas vincadas tipo origami.
- **Coerência Estrita e Fidelidade Total 3D ↔ 2D (BOM Real & Contagem Exata de Placas)**:
  - Todo componente físico visível no 3D **DEVE** ter sua representação 2D fiel em geometria, dimensões pós-queima e **quantidade exata de placas de corte (BOM 1:1)**:
    - **Proibido "amostras parciais" falsas**: se o objeto 3D é composto por $N$ faces (ex.: 10 faces superiores e 10 faces inferiores em um corpo facetado decagonal), a planificação 2D **NÃO PODE** exibir apenas 2 ou 3 trapézios com texto genérico "(10×)" esperando que o artesão adivinhe o arranjo. O plano de corte A4 DEVE conter **todas as 10 faces superiores e 10 faces inferiores** (dispostas em ninhos 1:1 sem colisão, ou planificadas como meias-fitas contínuas articuladas por vincos), além de fundo, bicos e alças.
    - **Corpo / Estrutura**: cilindro reto $\rightarrow$ retângulo ($P \times H$); tronco de cone $\rightarrow$ setor anular; parede inclinada plana $\rightarrow$ trapézio isósceles real; base/fundo oval ou chanfrado $\rightarrow$ contorno real correspondente (elipse/cápsula/polígono), nunca substituído por retângulo genérico quando a forma no 3D for curva.
    - **Tampas & Encaixes**: disco externo no diâmetro real + indicação de plugue/rebaixo ($\Delta\varnothing = 2.0\text{ mm}$) e respiro/puxador quando existentes no 3D.
    - **Alças, Bicos & Pés**: comprimento desenvolvido real da curva/perfil no 3D (calculado ao longo do caminho 3D) e largura exata da tira, bico cônico em setor anular e pé anelar com perímetro fechado compatível com o diâmetro da base.
- **Zero Sobreposição e Limite Estreito da Folha A4 (`297 × 210 mm`)**:
  - **Proibido terminantemente** que qualquer molde, placa, cota ou rótulo se sobreponha a outro elemento na mesma folha A4, ou ultrapasse a área útil `x ∈ [4, 293] mm`, `y ∈ [14, 205] mm`.
  - Margem livre de segurança mínima entre qualquer par de moldes ou textos: **$6.0\text{ mm}$**.
  - Setores largos ou conjuntos multi-placas devem ser distribuídos em **folhas A4 adicionais** (2, 3, 4 ou mais folhas A4), nunca espremidos ou sobrepostos.

### 3. Ângulo de Chanfro (Bisel) e Faixa de Desbaste ($w$)
Em junções de placas em ângulo $\alpha$:
$$\theta_{\text{bisel}} = \frac{180^\circ - \alpha}{2}$$
Para junções de 90°: $\theta = 45^\circ$. A largura da faixa horizontal raspada de corte é:
$$w = E \times \tan(\theta)$$
*(Ex: placa de $E = 6\text{ mm}$ a $45^\circ \implies w = 6.0\text{ mm}$; a $30^\circ \implies w = 3.5\text{ mm}$)*.

### 4. Ancoragem Realista com Barbotina e Gabaritos 1:1
- Raízes de alças e bicos com concavidade correspondente ao raio da parede receptora.
- Retângulos de barbotina pontilhados com cotas exatas de altura na placa do corpo.
- Gabarito lateral de curvatura 1:1 acompanhando a tira reta esticada.

### 5. Tolerância de Encaixe para Tampas e Pés ($\Delta\varnothing$)
Compensação de retração de queima (10% a 15%) e camada de esmalte ($0.5\text{ a }1.0\text{ mm}$):
$$\varnothing_{\text{plug\_tampa}} = \varnothing_{\text{boca\_interna}} - 2.0\text{ mm}$$
Largura da galeria de apoio: $w_{\text{galeria}} \ge E_{\text{tampa}}$.

---

## 3. Estrutura Obrigatória da Resposta

Sempre responda estruturando rigorosamente as 4 seções abaixo:

### PARTE 1: DIAGNÓSTICO DE ENTRADA & ENGENHARIA REVERSA
- **Origem da Análise**: Identificar se a entrada foi *Texto*, *Foto de Peça Pronta (3D)* ou *Imagem de Molde (2D)*.
- **Leitura Visual / Morfológica**: Detalhar o que foi identificado visualmente (linhas de dobra, espessura aparente de parede, proporções $H/\varnothing$, simetria, acabamento cerâmico).

### PARTE 2: CONCEITO ESTÉTICO & PROMPT VISUAL
- **Linguagem & Filosofia**: Conceito formal de ateliê (Wabi-Sabi, Stoneware Nórdico, Terracota Brutalista, Porcelana Minimalista).
- **Prompt de Imagem (Inglês)**: Prompt refinado para Midjourney / DALL-E / Imagen com iluminação zen de estúdio cerâmico, textura tátil de argila, linhas de dobra vincadas visíveis e perspectiva isométrica.

### PARTE 3: MEMORIAL DE ENGENHARIA CERÂMICA
- Tabela de decomposição anatômica nos blocos do app.
- Cálculos explícitos de diâmetros, ângulos de bisel ($\theta$), largura da faixa de corte ($w$), retração e folgas de encaixe ($\Delta\varnothing$).

### PARTE 4: ESPECIFICAÇÃO DE DADOS (CeramiCAD JSON SPEC)
Gere o bloco de código JSON pronto para ser colado ou carregado no CeramiCAD Studio Pro:

#### Formato 1: Peças de Placas Dobradas / Origami Cerâmico (Monoplaca ou Multipeças)
```json
{
  "key": "identificador_snake_case",
  "title": "#XX — Nome da Peça em Placas",
  "category": "decoracao|mesa|cozinha|historico",
  "method": "plaqueira|especial",
  "dimensions": { "length": 240, "width": 60, "height": 30, "thickness": 5.5 },
  "patterns": [
    {
      "label": "Placa Principal",
      "outline": [
        { "x": -100, "y": -30 }, { "x": 100, "y": -30 },
        { "x": 100, "y": 30 }, { "x": -100, "y": 30 }
      ],
      "creases": [
        { "p1": { "x": -100, "y": -10 }, "p2": { "x": 100, "y": -10 }, "type": "valley", "angleDeg": 30 },
        { "p1": { "x": -100, "y": 10 }, "p2": { "x": 100, "y": 10 }, "type": "valley", "angleDeg": 30 }
      ],
      "bevels": [
        { "edgeIndex": 0, "angleDeg": 45, "widthMm": 5.5 }
      ],
      "anchors": []
    }
  ]
}
```

#### Formato 2: Peças Cilíndricas / Cônicas Modulares (Olaria de Estágios)
```json
{
  "key": "identificador_snake_case",
  "title": "Nome do Utilitário Modular",
  "stages": [
    { "type": "base", "diameter": 10.0, "thickness": 0.6, "bevelDeg": 45 },
    { "type": "cone", "dBase": 10.0, "dTopo": 14.0, "height": 6.0, "thickness": 0.6 },
    { "type": "cylinder", "diameter": 14.0, "height": 4.0, "thickness": 0.6 },
    { "type": "handle", "profile": "fita", "width": 2.2, "projection": 4.5, "yBase": 2.0, "yTopo": 8.0 },
    { "type": "lid", "diameter": 14.0, "plugDiameter": 12.6, "thickness": 0.6 }
  ]
}
```

---

## 4. Modo Lote & Coleções (Batch Processing em Documento Único)

A habilidade pode processar **múltiplas peças simultaneamente** (ex: coleções completas como Jogo de Café: bule + xícara + pires + açucareiro; trio de vasos P/M/G; conjunto de luminárias) e consolidar **todas em um único documento** (`.json` ou `.md`).

### Diretrizes de Coerência para Coleções
1. **Coerência Dimensional e de Encaixe Inter-Peças**:
   - Rebaixo do pires casa com a base da xícara: $\varnothing_{\text{rebaixo\_pires}} = \varnothing_{\text{base\_xicara}} + 1.0\text{ a }1.5\text{ mm}$.
   - Tampas de diferentes recipientes (bule, açucareiro) obedecem às mesmas regras de folga ($\Delta\varnothing = 2\text{ mm}$) e pegadores com mesmo design system.
2. **Identidade Visual e Material Unificada**:
   - Mesma espessura nominal de placa ($E$), mesma retração de massa (ex: 12%) e paleta coerente de esmaltes.

### Formato de Saída Consolidada (Arquivo JSON Único de Catálogo)
Quando solicitado para salvar em lote ou em arquivo único, estrutura o documento como um catálogo completo:
```json
{
  "collection_id": "jogo_cafe_minimalista",
  "collection_title": "Coleção Café Origami Contemporâneo",
  "author": "Ateliê CeramiCAD",
  "shrinkage_rate": 0.12,
  "pieces": {
    "bule_origami": {
      "key": "bule_origami",
      "title": "#01 — Bule Origami 600ml",
      "category": "mesa",
      "dimensions": { "length": 180, "width": 120, "height": 140, "thickness": 5.5 },
      "patterns": [ ... ]
    },
    "xicara_espresso": {
      "key": "xicara_espresso",
      "title": "#02 — Xícara de Espresso 80ml",
      "category": "mesa",
      "dimensions": { "length": 70, "width": 70, "height": 55, "thickness": 4.5 },
      "patterns": [ ... ]
    },
    "pires_facetado": {
      "key": "pires_facetado",
      "title": "#03 — Pires Facetado Hexagonal",
      "category": "mesa",
      "dimensions": { "length": 120, "width": 120, "height": 15, "thickness": 4.5 },
      "patterns": [ ... ]
    }
  }
}
```
*Se o usuário solicitar a gravação em disco, o agente deve gravar diretamente no caminho especificado (ex: `catalogos/minha_colecao.json` ou `docs/colecao_moldes.md`).*


---

## 5. Padrão de Visualização e Detalhamento Mínimo (CeramiCAD Viewer)

Ao gerar visualizadores HTML para catálogos ou lotes:
1. **Expositor em Tela Cheia com Multi-Seleção dos 3 Visualizadores**:
   - **Palco + Sidebar Retrátil**: canvas/palco ocupando 100% da viewport com barra lateral flutuante recolhível.
   - **Multi-Seleção com Mínimo 1 Ativo**: o usuário pode ligar/desligar individualmente os 3 visualizadores (**3D Sólido**, **3D Esqueleto de Placas**, **Moldes A4 1:1**). Com 1 ativo ele ocupa 100% do palco; com 2 ou 3 ativos o palco divide automaticamente lado a lado.
   - **Fidelidade Total do Esqueleto (`buildPlates`) e Vista Explodida (`isExploded`)**:
     - O esqueleto de placas deve ser derivado diretamente das malhas/arestas reais de `buildSolid` (ou espelhar 100% dos subcomponentes, posições e transformações — incluindo conjuntos multi-peças, azulejos, alças, bicos e tampas), nunca aproximações genéricas que omitam partes da peça.
     - Quando `isExploded` estiver ativo, **todas** as placas/componentes do item devem se separar radial ou axialmente de forma coerente tanto no 3D Sólido quanto no Esqueleto.
2. **Navegação por Lotes**: Agrupar botões por Lote (`Lote 1`, `Lote 2`, ...) exibindo `Código • Nome` da peça.
3. **Encaixe 3D Hermético em Meia-Esquadria (Proibido `BoxGeometry` Rotacionado em Paredes Inclinadas)**:
   - Em peças poligonais com paredes inclinadas (ex.: formas de pão, vasos facetados, caixas trapezoidais), **NUNCA** use `THREE.BoxGeometry` rotacionado, pois blocos retangulares inclinados abrem frestas em "V" nos cantos e se interceptam na base.
   - Construa cada parede e o fundo chanfrado usando um **hexaedro exato de 8 vértices** (`buildHexahedronGeo(pts8)`), compartilhando as mesmas coordenadas 3D de aresta interna e externa `(xOB, zOB, yOB)`, `(xIB, zIB, yIB)`, `(xOT, zOT, yOT)`, `(xIT, zIT, yIT)` para fechamento em meia-esquadria com erro $0.00\text{ mm}$.
   - Raízes de alças e bicos devem penetrar de $1.0\text{ a }1.5\text{ mm}$ na parede externa receptora na altura exata do perfil ($r(y)$) para eliminar frestas visuais.
4. **Escala Real 1:1 Inegociável e Zero Sobreposição no A4 (`1 unidade SVG = 1 mm`)**:
   - **Proibido reduzir escala (`0.7x`, `1:2`)** para forçar moldes dentro de uma folha A4. Todo molde deve ter tamanho real 1:1 pós-queima.
   - Quando uma tira ou malha exceder a largura útil da folha A4 ($297 \times 210\text{ mm}$), divida a peça em **meias-tiras ou meias-malhas em escala 1:1** com faixa de bisel/aba de união explícita, ou distribua os componentes em **2, 3 ou mais folhas A4**.
   - **Proibido sobrepor peças**: Nunca aninhe discos dentro do raio interno de setores cônicos se houver colisão com a linha de corte, hachura de bisel ou cotas. Garanta margem livre mínima de $8\text{ mm}$ entre qualquer par de moldes.
   - Em discos pequenos ($r \le 18\text{ mm}$) ou crivos perfurados, posicione o título acima (`cy - r - 4.2`) e o subtítulo abaixo (`cy + r + 5.2`) do círculo para não colidir com furos ou linhas de cota.
5. **Alinhamento de Vértices Poligonais e Paginação SVG Multi-Folhas**:
   - **Proibido misturar `CylinderGeometry` em fundos/tampas poligonais**: `THREE.CylinderGeometry` usa fase `+Z` (`sin/cos`), enquanto `buildThickRevolvedProfile`, `buildHollowCylinder` e `buildHollowConeFrustum` usam fase `+X` (`cos/sin`). Construa fundos/tampas/plugs poligonais com `buildThickRevolvedProfile([[0, y0], [r, y0], [r, y1], [0, y1]], segs)` para garantir alinhamento exato de arestas.
   - **Agrupamento de folhas A4**: envolva cada folha `sN` em `<g transform="translate(0, (N-1)*225)"> ... </g>`, pois `a4SheetFrame()` fecha seu próprio `</g>` interno.
6. **Acabamento Escultural Refinado (Proibido Geometria Bruta / "Blocões")**:
   - Antes de propor um novo lote, liste os `shortName`, perfis e dimensões já existentes no catálogo para garantir 0% de repetição temática, nominal ou geométrica (nenhum par de códigos pode compartilhar a mesma peça/geometria).
   - **Perfis orgânicos multi-pontos**: peças torneadas ou bojudas devem usar `buildThickRevolvedProfile` com silhueta suave (13 a 25+ pontos), incluindo **anel de pé recuado**, transição contínua de bojo/ombro/gargalo e **bainha ou lábio arredondado** na boca — nunca pilhas secas de 4–6 pontos.
   - **Bases chanfradas e aberturas reais**: bases retangulares/cápsulas devem ter extremidades arredondadas e bisel via `ExtrudeGeometry`; suportes vazados (rechauds, lanternas) devem ter pilares e janelas realmente abertas, nunca blocos fechados com manchas escuras simuladas.
   - **Arcos, alças e encaixes**: nunca use `BoxGeometry` solto para braços curvos ou nervuras cônicas; use `buildThickStripPath3D` (com gabarito 1:1 no A4) para alças/arcos Bézier e `buildHexahedronGeo` alinhado à geratriz do cone para janelas vazadas. Em peças modulares empilháveis, prefira **encaixe anelar macho-fêmea** ($\Delta\varnothing = 2.0\text{ mm}$).
7. **Detalhes Funcionais Cerâmicos & Unidades de UI (`cm` vs `mm`)**:
   - **Escoamento de líquidos**: coadores, funis e peças com furo inferior de escoamento devem incluir **pesinho circular gotejador** saliente na face inferior ao redor do orifício para evitar que o líquido escorra lateralmente para fora do recipiente.
   - **Unidades na UI**: controles editáveis de altura/dimensão para o usuário na interface do Viewer devem ser apresentados e editados em **centímetros (`cm`)**, mantendo internamente `1 unidade SVG = 1 mm` nas folhas A4 1:1 e exportação PDF/impressão.
8. **Protocolo Obrigatório de Segunda Verificação (Dual Verification & BOM Parity Protocol)**:
   - **Nunca considerar uma peça ou lote pronto apenas por passar com `CONSOLE_ERRORS: 0`**.
   - **Fase 1 — Verificação Sintática e Funcional**:
     - Carga limpa no navegador headless, ausência de exceções JS ou erros de console Three.js.
   - **Fase 2 — SEGUNDA VERIFICAÇÃO GEOMÉTRICA, BOM E ANTI-SOBREPOSIÇÃO**:
     - **Paridade Estrita de Corte (BOM 3D ↔ 2D 1:1)**:
       - Toda placa física necessária para erguer a peça no ateliê DEVE estar desenhada e cortável nas folhas A4 (ou o gabarito sanfonado completo contendo todas as faces numeradas com vincos). É proibido desenhar amostras parciais ("1B. Cópia Ref.") quando o modelo exigir múltiplas peças (ex.: 10 faces superiores e 10 faces inferiores exigem todas as 10 faces de cada banda desdobradas nas folhas A4).
       - A contagem de placas sincronizadas no Esqueleto (`plateCount`) DEVE refletir a decomposição física real (ex.: 23 placas no regador facetado, e não 5 agrupadas).
     - **Detector Automatizado de Colisão e Sobreposição 2D (Zero Overlap Check)**:
       - Executar varredura algorítmica de Bounding Boxes (`getBBox()`) e envelopes de colisão em todas as geometrias e textos de cada folha SVG.
       - Acusar erro impeditivo se a distância entre quaisquer duas geometrias ou textos/cotas for menor que $6.0\text{ mm}$, ou se houver qualquer área de sobreposição.
     - **Confinamento Rígido de Margens da Folha A4**:
       - Garantir que nenhum elemento encoste ou ultrapasse a faixa de cabeçalho (`y < 14 mm`), rodapé (`y > 205 mm`) ou bordas laterais (`x < 4 mm` ou `x > 293 mm`).
     - **Inspeção Visual Tríplice (Headless Screenshot)**:
       - Inspecionar visualmente os 3 painéis: (1) 3D Sólido Anatômico, (2) Esqueleto Marfim de Alta Legibilidade e (3) Moldes A4 1:1.
       - Se qualquer elemento colidir, sobrepor ou faltar na lista de corte, o lote é REJEITADO imediatamente e retorna para refatoração.
   - **Fase 3 — SEGUNDA VERIFICAÇÃO OBRIGATÓRIA DE CONTROLES INTERATIVOS & RESPONSIVIDADE**:
     - **A) Comutação Universal de Cor e Material (`applyActiveMaterialToGroup`)**:
       - O novo código DEVE responder dinamicamente à troca de acabamentos da paleta de materiais (`Terracota`, `Verde Sálvia`, `Areia Crua`, `Ardósia Esmaltada`, `Modo Técnico`).
       - É proibido instanciar malhas de argila com materiais fixos locais isolados que ignorem o seletor. Todas as malhas cerâmicas devem aceitar `mat.outer`, `roughness`, `metalness` e `wireframe`.
       - Elementos não-cerâmicos (vidros de tubos de ensaio com `opacity < 0.85`, água transparente, recipientes de tealight de metal, pavio e caules/flores decorativas) DEVEM ser protegidos e preservar seus materiais originais intactos.
     - **B) Controle Paramétrico de Altura/Escala & Auto-Paginação Dinâmica (`DynamicSheetManager`)**:
       - É proibido bloquear modelos 3D com `isFlatTile: true` por terem originalmente 1 folha A4 (`sheets: 1`). Apenas ladrilhos/azulejos planos reais (`basePieceHeightMm <= 16` e categoria azulejos) podem ter a altura travada.
       - Ao submeter a peça a estresse de escala ($1.5\times$ e $2.0\times$) através de `setPieceHeightCm()`:
         - A malha 3D e o esqueleto devem escalar em $Y$.
         - Peças expandidas que excederem o espaço útil DEVEM ser migradas automaticamente para folhas A4 adicionais (`Folha #N+1`).
         - Placas que excederem a capacidade do A4 DEVEM sofrer **Split Tiling** contínuo com aba de sobreposição de $15\text{ mm}$ e duas marcas em cruz de precisão ($\oplus$).
         - Nenhuma colisão ou sobreposição pode ocorrer sob qualquer fator de escala.
     - **C) Corte Seccional ½ (Inspeção Interna de Parede)**:
       - O modelo DEVE responder ao comando de corte seccional (`isSectionCut = true`). Se não possuir recorte geométrico nativo com tampas, o Three.js DEVE aplicar o plano de corte de precisão (`sectionClippingPlane` em $Z=0$), expondo instantaneamente a espessura da parede interna.
     - **D) Modo Explodir Sincronizado**:
       - Ao ativar `isExploded = true`, todos os subcomponentes múltiplos, tampas e alças devem se afastar radial ou axialmente sem colisões.
     - **E) Execução Obrigatória de Testes Automatizados**:
       - Sempre que novos objetos forem adicionados a `PIECES[]`, é **obrigatório** rodar no terminal e obter 100% de aprovação:
         ```bash
         node tests/second_verification.js --range <inicio>-<fim>
         ```
       - O script executará automaticamente todas as validações das Fases 1, 2 e 3 para os novos códigos. Se qualquer item falhar, o código deve ser corrigido antes de finalizar a entrega.


9. **Diretrizes de Empacotamento 2D A4 e Zero Folhas Fantasmas (`DynamicSheetManager`)\**:
   - **Preservação Sagrada do Layout Nominal ($k = 1.0$)**:
     - Quando em escala padrão 1.0x (ou para modelos que caibam nas folhas sem colisão), o layout original do artesão DEVE ser preservado 100% inalterado. Nenhuma reorganização dinâmica atua sem necessidade real.
   - **Shelf / Bin Packing 2D em Múltiplas Colunas**:
     - Quando a altura for expandida ($k > 1.0$), o algoritmo deve priorizar o preenchimento de colunas laterais na **mesma folha A4** ($W_{\text{livre}} \le 286\text{ mm}$), acomodando peças pequenas lado a lado com peças maiores antes de criar folhas adicionais.
   - **Fatiamento Limpo (Split Tiling Exclusivo)**:
     - É **proibido** embutir marcações de emenda ou divisões prematuras em funções básicas de desenho (`drawAnnotatedRect`). O fatiamento contínuo (com abas de 15 mm e alinhadores em cruz ⊕) deve ser realizado exclusivamente pelo `DynamicSheetManager` para placas que excederem o tamanho físico do A4 ($H > 172\text{ mm}$).
   - **Filtro Anti-Fantasma**:
     - Nenhuma folha A4 pode ser emitida se contiver apenas linhas de emenda, miras de corte ou textos órfãos sem corpo cerâmico real ($A \ge 100\text{ mm}^2$).

10. **Protocolo de Sincronização com o Repositório de Produção (`CeramiCAD`)\**:
   O ecossistema CeramiCAD é composto por dois repositórios irmãos:
   1. **`cerâmiKa`** (`C:\Users\bruno\Documents\cerâmiKa`):
      - Ambiente de desenvolvimento, prototipagem, testes de estresse, auditorias PDCA e visualizadores parciais/lotes.
   2. **`CeramiCAD`** (`C:\Users\bruno\Documents\CeramiCAD`):
      - Repositório de distribuição pública e deploy online no GitHub Pages (`CeramiCAD.html` autocontido + `README.md`).

   **Regra de Sincronização**:
   Sempre que o usuário solicitar *"atualize o CeramiCAD"*, *"publique a versão de produção"* ou referenciar `C:\Users\bruno\Documents\CeramiCAD`:
   1. Copiar `cerâmiKa/docs/projetos/catalogo_viewer.html` → `CeramiCAD/CeramiCAD.html`, ajustando o `<title>` para `CeramiCAD — Expositor 3D & Moldes A4 1:1 (<N> Modelos • <M> Lotes)`.
   2. Atualizar o `README.md` de `CeramiCAD` com a quantidade de peças, número de lotes e a tabela completa das novas coleções.
   3. Executar verificação automatizada via CDP Headless Chromium em `CeramiCAD.html`.
   4. Criar commit local e tag de versão (ex: `v2.0`).
   5. **Portão de Deploy**: Notificar o usuário para executar o `git push origin main --tags` no terminal do Windows (onde as credenciais do GitHub já estão salvas).
