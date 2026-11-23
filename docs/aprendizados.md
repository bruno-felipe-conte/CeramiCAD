# 🏺 Diretrizes de Engenharia e Aprendizados Técnicos — CeramiCAD

Este documento consolida os aprendizados, decisões arquiteturais e regras matemáticas do **CeramiCAD** acumulados ao longo dos ciclos de desenvolvimento e refinamento no ateliê.

---

## 1. As 5 Regras Matemáticas Inegociáveis de Manufatura Cerâmica

1. **Continuidade de Diâmetro nas Juntas (Casamento Estanque)**:
   $$\varnothing_{\text{topo (Estágio } N)} = \varnothing_{\text{base (Estágio } N+1)}$$
   Garante perímetro idêntico de união ($P = \pi \times \varnothing$) sem degraus ou pontos fracos na queima.

2. **Desenvolvibilidade 2D de Placas (*Slab Building*)**:
   - *Cilindro*: retângulo plano ($P \times H$).
   - *Tronco de Cone*: setor anular curvo com $R = \frac{r_{\max} \cdot s}{r_{\max} - r_{\min}}$, $r = R - s$, $\theta = \frac{360^\circ \cdot r_{\max}}{R}$.
   - *Paredes Inclinadas / Pirâmides*: trapézios isósceles desenhados pela geratriz real $s = \frac{\Delta y}{\sin\alpha}$, nunca retângulos genéricos.
   - *Paridade Estrita 3D ↔ 2D (BOM 1:1)*: todas as peças do 3D devem estar desdobradas nos moldes A4 (proibido amostras parciais).

3. **Ângulo de Chanfro (Bisel) e Faixa de Desbaste ($w$)**:
   $$\theta_{\text{bisel}} = \frac{180^\circ - \alpha}{2}, \quad w = E \times \tan(\theta)$$
   Fundamental para junções herméticas em meia-esquadria sem frestas na argila crua.

4. **Ancoragem Realista com Barbotina**:
   - Raízes de alças e bicos com concavidade correspondente ao raio da parede receptora.
   - Marcação de áreas de escarificação pontilhadas e gabarito lateral de curvatura 1:1.

5. **Tolerância de Encaixe para Tampas e Pés ($\Delta\varnothing$)**:
   $$\varnothing_{\text{plug\_tampa}} = \varnothing_{\text{boca\_interna}} - 2.0\text{ mm}$$
   Compensa a retração de queima (Cone 6: $r = 12\%$) e a camada de esmalte vitrificado.

---

## 2. Motor 2D de Paginação e Zero Folhas Fantasmas (`DynamicSheetManager`)

1. **Preservação Sagrada do Layout Nominal ($k = 1.0$)**:
   - Na escala padrão ou para peças que caibam perfeitamente na prancha original, o layout concebido pelo artesão permanece 100% inalterado.
2. **Shelf / Bin Packing 2D em Múltiplas Colunas**:
   - Ao expandir a altura ($k > 1.0$), o algoritmo preenche a largura lateral livre da folha A4 ($W_{\text{livre}} \le 286\text{ mm}$), aninhando peças menores lado a lado antes de abrir páginas extras.
3. **Split Tiling Centralizado e Limpo**:
   - Placas que excedem a capacidade física do A4 ($H > 172\text{ mm}$) são fatiadas em segmentos contínuos com aba de sobreposição de $15\text{ mm}$ e alinhadores em cruz ($\oplus$).
   - O fatiamento é proibido nas funções primitivas de desenho (`drawAnnotatedRect`) para evitar fragmentos órfãos.
4. **Filtro Anti-Fantasma**:
   - Nenhuma folha A4 é gerada se contiver apenas linhas de emenda ou cotas soltas sem malha cerâmica real ($A \ge 100\text{ mm}^2$).

---

## 3. Controles Universais de Bancada e Inspeção 3D

1. **Corte Seccional ½ por Clipping Plane Three.js**:
   - Plano de corte universal em $Z=0$ com `renderer.localClippingEnabled = true`, permitindo inspecionar a espessura de parede e encaixes internos de 100% dos modelos.
2. **Paleta Cerâmica PBR Inteligente**:
   - Propagação de acabamentos (Terracota, Verde Sálvia, Areia Crua, Ardósia Esmaltada) nas malhas de argila, preservando intactos elementos não-cerâmicos (tubos de ensaio de vidro, água, partes metálicas e flores).
3. **Redimensionamento Paramétrico Desbloqueado**:
   - Modelos tridimensionais (mesmo os que iniciam em 1 folha A4) têm sliders e inputs de altura totalmente funcionais, recalculando volume e repaginando moldes em tempo real.

---

## 4. Arquitetura de Repositórios Duplos

O ecossistema é organizado em dois repositórios complementares:
- **`cerâmiKa`** (`C:\Users\bruno\Documents\cerâmiKa`): ambiente de desenvolvimento, testes de estresse com Chromium headless CDP, caderno de projetos e ciclo PDCA.
- **`CeramiCAD`** (`C:\Users\bruno\Documents\CeramiCAD`): distribuição pública autocontida em arquivo único (`CeramiCAD.html` + `README.md`) para hospedagem direta no GitHub Pages.

### Protocolo de Deploy:
1. Copiar `cerâmiKa/docs/projetos/catalogo_viewer.html` $\rightarrow$ `CeramiCAD/CeramiCAD.html` com ajuste de `<title>`.
2. Atualizar badges, contagem de modelos e tabela de lotes no `README.md`.
3. Validar sintaxe e integridade via testes automatizados.
4. Realizar o commit e tag localmente no repositório.
5. Executar o push seguro via PowerShell no Windows:
   ```powershell
   git -C C:\Users\bruno\Documents\CeramiCAD push origin main --tags
   ```
