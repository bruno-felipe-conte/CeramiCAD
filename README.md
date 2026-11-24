<div align="center">

# 🏺 CeramiCAD

**Expositor 3D Interativo & Gerador de Moldes A4 1:1 para Cerâmica em Placas (*Slab Building*)**

[![Modelos](https://img.shields.io/badge/Cat%C3%A1logo-247%20Pe%C3%A7as-a3b8a8?style=for-the-badge)](#cat%C3%A1logo-completo-46-lotes)
[![Lotes](https://img.shields.io/badge/Cole%C3%A7%C3%A3o-46%20Lotes-c97e56?style=for-the-badge)](#cat%C3%A1logo-completo-46-lotes)
[![Escala A4](https://img.shields.io/badge/Moldes%202D-A4%201%3A1%20Real-4f7cac?style=for-the-badge)](#exporta%C3%A7%C3%A3o--impress%C3%A3o-11)
[![Arquivo %C3%9Anico](https://img.shields.io/badge/Deploy-HTML%20%C3%9Anico%20Autocontido-1f242d?style=for-the-badge)](#como-usar)

### 🌐 [Abrir CeramiCAD Online (GitHub Pages)](https://bruno-felipe-conte.github.io/CeramiCAD/CeramiCAD.html)

</div>

---

## ✨ Visão Geral

O **CeramiCAD** é uma aplicação web **100% autocontida em um único arquivo HTML** ([`CeramiCAD.html`](./CeramiCAD.html)) para projetar, inspecionar e planificar peças cerâmicas construídas por placas (*slab building*).

Cada uma das **247 peças** do catálogo combina modelagem paramétrica em Three.js, esqueleto estrutural sincronizado e pranchas vetoriais **em escala real 1:1 (297 × 210 mm)** prontas para impressão, corte na argila e exportação em **PDF** ou **STL**.

---

## 🎯 Principais Recursos

### 1. Expositor em Tela Cheia com 3 Visualizadores Selecionáveis
Escolha livremente quais visualizadores exibir na barra lateral retrátil (mínimo 1 ativo):
- **3D Sólido**: modelo anatômico com espessura real de parede ($E$), pés recuados, biséis, tampas com folga de encaixe ($\Delta\varnothing = 2{,}0\text{ mm}$), alças e bicos.
- **Esqueleto de Placas 3D**: visualização em arame estrutural derivada diretamente da geometria 3D, destacando anéis, costuras, aberturas e subpeças.
- **Moldes A4 1:1**: planificação técnica em folhas A4 (`1 unidade SVG = 1 mm`) com linhas de corte, hachura de bisel/chanfro, marcas de barbotina, cotas em milímetros e seletor individual por folha.

> **Palco Adaptativo**: com **1** visualizador selecionado ele ocupa **100% da tela**; com **2 ou 3** ativos, o palco divide automaticamente lado a lado com câmeras 3D sincronizadas.

### 2. Controles Interativos de Bancada
- **Vista Explodida**: separa todas as placas, fundos, tampas, alças, bicos e acessórios para inspeção da montagem.
- **Corte Seccional (½) por Clipping Plane Universal**: abre qualquer geometria ao meio em $Z=0$ revelando espessuras internas, galerias e encaixes.
- **Paleta Cerâmica PBR Inteligente**: alternância instantânea entre argilas cruas e esmaltes vidrados, preservando acessórios transparentes (tubos de ensaio) e detalhes botânicos.
- **Redimensionamento Paramétrico Dinâmico de Altura**: sliders de altura reativos que recalculam o volume Three.js e repaginam automaticamente os moldes 2D.
- **Shelf / Bin Packing 2D em Múltiplas Colunas**: algoritmo inteligente que agrupa peças pequenas lado a lado na mesma folha A4 e executa *split tiling* contínuo (com abas de 15 mm e miras $\oplus$) apenas para peças maiores que a folha, eliminando 100% de folhas fantasmas ou linhas órfãs.
- **Exportação Direta**:
  - **PDF A4 1:1** multi-folhas pronto para imprimir sem redimensionamento (`jsPDF`).
  - **STL 3D** para impressão 3D de gabaritos/protótipos (`STLExporter`).

---

## 📐 Princípios de Engenharia Cerâmica Aplicados

1. **Desenvolvibilidade 2D Exata**: cilindros como retângulos ($P \times H$), troncos de cone como setores anulares ($R - r = s$, $\theta = f \cdot \frac{360^\circ (r_{\max} - r_{\min})}{s}$), pirâmides inclinadas como trapézios isósceles pela geratriz real.
2. **Escala Real 1:1 Pós-Queima**: calibração para Cone 6 ($r = 12\%$) sem redução artificial de escala — peças maiores são divididas em meias-placas ou múltiplas folhas A4 com abas de emenda calibradas.
3. **Encaixes em Meia-Esquadria e Tolerâncias**: paredes poligonais fechadas por hexaedros exatos sem frestas e tampas com folga $\Delta\varnothing = 2{,}0\text{ mm}$.
4. **Cotas de Bisel ($w = E \cdot \tan\theta$)**: faixas de desbaste e áreas de escarificação/barbotina indicadas diretamente nos moldes.

---

## 🗂️ Catálogo Completo (46 Lotes • 247 Modelos)

| Lotes | Faixa de Códigos | Temas & Tipologias |
| :--- | :--- | :--- |
| **Lotes 1 – 5** | `001` – `025` | Bules, chawans, cuias, gaiwan, xícaras, pratos, vasos cilíndricos e utilitários de chá |
| **Lotes 6 – 10** | `026` – `050` | Castiçais, luminária pagode, bule hexagonal, jarras assimétricas, regador, pilão suribachi |
| **Lotes 11 – 15** | `051` – `075` | Galheteiro, tigela de tricô, espremedor, difusor, molheira, kyusu lateral, leiteira, boleira |
| **Lotes 16 – 20** | `076` – `100` | Decanters, moringa, garrafa Bordeaux, fruteira plissada, jogo Ikigai, bandeja, vaso orbital |
| **Lotes 21 – 25** | `101` – `119` | Xícaras hexagonais, luminárias facetadas/torres, cálice, travessas, bule poliédrico, lanternas |
| **Lotes 26 – 30** | `120` – `155` | Azulejaria e tesselação geométrica, porta-retratos duplos, arandelas, nichos e cerâmica costurada |
| **Lotes 31 – 35** | `156` – `190` | Peças escultóricas, ondas senoidais, vasos em arco, molduras de propagação sálvia/mostarda, pirâmides e suportes |
| **Lotes 36 – 40** | `191` – `215` | Vasos de tubos múltiplos, floreira retangular canelada, regador diamante facetado, conjuntos Madar, galheteiros, Sizuku & porta-guardanapos |
| **Lotes 41 – 46** | `216` – `245` | Travessas, cantis, garrafas, trio Neuralsip, moringa, jarras rústicas/disco/argolas, manteigueiras francesas/retangulares, petisqueira modular 4×, porta-guardanapos temáticos & saleiro articulado |

---

## 🚀 Como Usar

Não requer instalação, build ou servidor complexo:

1. **Abrir diretamente**: dê um duplo clique em [`CeramiCAD.html`](./CeramiCAD.html) no seu navegador moderno (Chrome, Edge, Firefox ou Safari).
2. **Ou servir localmente**:
   ```bash
   python3 -m http.server 8099
   # Acesse: http://localhost:8099/CeramiCAD.html
   ```

> **Dica de Impressão A4 1:1**: ao imprimir ou gerar o PDF, mantenha a escala do navegador/leitor de PDF em **100% (Tamanho Real)** — nunca *"Ajustar à página"* — e confira a barra de escala de **50 mm** presente em todas as folhas.

---

## 🖱️ Controles do Expositor 3D

| Ação | Mouse / Trackpad |
| :--- | :--- |
| **Orbitar (girar)** | Botão esquerdo + arrastar |
| **Zoom** | Roda do mouse / pinça |
| **Mover (Pan)** | Botão direito + arrastar |

---

## 📚 Documentação Técnica & Aprendizados de Engenharia

Para consultar os princípios matemáticos de modelagem, regras de corte, o algoritmo de *Bin Packing 2D* em múltiplas colunas e a arquitetura do projeto:
- 📖 [Diretrizes de Engenharia e Aprendizados Técnicos (`docs/aprendizados.md`)](./docs/aprendizados.md)
- 🧠 [Skill Arquitetural CeramiCAD para Agentes de IA (`.agents/skills/ceramic-3d-architect/SKILL.md`)](./.agents/skills/ceramic-3d-architect/SKILL.md)

