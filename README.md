# 🚀 Simulação de Trajetória em Tubo de Raios Catódicos (TRC)
### **Instituto de Física da Universidade de São Paulo (IFUSP)**
#### *Experimento 2 - Atividade 1 | Disciplina: 4302213 - Física Experimental III*

<div align="center">

[![Python 3.10+](https://img.shields.io/badge/python-3.10%2B-blue.svg?style=for-the-badge&logo=python&logoColor=white)](https://www.python.org/)
[![Jupyter Notebook](https://img.shields.io/badge/tools-Jupyter-orange.svg?style=for-the-badge&logo=jupyter&logoColor=white)](https://jupyter.org/)
[![FEMM](https://img.shields.io/badge/simulation-FEMM-green.svg?style=for-the-badge)](https://www.femm.info/)
[![IFUSP](https://img.shields.io/badge/institution-IFUSP-red.svg?style=for-the-badge)](https://portal.if.usp.br/ifusp/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge)](https://opensource.org/licenses/MIT)

</div>

---

> 📘 **Sobre este texto:**  
> Este é o README do projeto. Aqui você encontra instruções de uso, explicações teóricas e o passo a passo da simulação.  
> Se estiver em dúvida sobre como rodar ou entender o experimento, comece lendo aqui.

---

## 🧾 TL;DR

Simulação numérica da trajetória de elétrons em um Tubo de Raios Catódicos (TRC), combinando dados de campo elétrico obtidos via FEMM com integração numérica em Python para modelar a deflexão do feixe e comparar com resultados experimentais.

---

## ⚠️ Comece por aqui (uso rápido para iniciantes)

Se você só quer rodar a simulação, **este é o único arquivo que você precisa baixar:**

📥 **Notebook principal:**  
[Exp2Atv1 simulacao_TRC Guterman.ipynb](https://github.com/gutermanjunior/Lab3_2026-Exp2Atv1-CRT_Simulation_Electric_Field/blob/main/Exp2Atv1%20simulacao_TRC%20Guterman.ipynb)

---

### 🔽 Como baixar o arquivo

1. Clique no link acima.
2. Na página do GitHub, clique no botão **"Download"** ou **"Raw"** (canto superior direito).
3. Salve o arquivo `.ipynb` no seu computador.

---

### ▶️ Como abrir no Google Colab (recomendado)

A forma mais simples de usar o notebook é pelo Google Colab (roda direto no navegador):

1. Acesse: https://colab.research.google.com/
2. Clique na aba **"Upload" (Enviar)**.
3. Selecione o arquivo `.ipynb` que você baixou.
4. Após abrir, vá em **Ambiente de execução → Executar tudo**.

💡 Não é necessário instalar Python nem bibliotecas — o Colab já vem pronto para uso.

---

### ✅ Alternativa (sem baixar)

Você também pode visualizar o notebook diretamente:

👉 GitHub Pages (mais interativo)  
👉 NBViewer (mais estável)

*(links disponíveis mais abaixo neste README)*

---

## 📋 Sumário
1. [Sobre o Projeto](#-sobre-o-projeto)  
2. [Contexto do Experimento](#-contexto-do-experimento)  
3. [Fundamentação Teórica](#️-fundamentação-teórica)  
4. [Fluxo de Trabalho (Pipeline)](#️-o-fluxo-de-trabalho-pipeline)  
5. [Funcionalidades](#-funcionalidades-do-notebook)  
6. [Estrutura do Repositório](#-estrutura-do-repositório)  
7. [Guia de Instalação e Uso](#-como-usar)  
8. [Resultados](#-exemplo-de-resultado)  
9. [Citação](#-como-citar)

---

## 📖 Sobre o Projeto
Simulação numérica e análise experimental da deflexão de elétrons em um Tubo de Raios Catódicos (TRC), integrando resultados de simulações por Elementos Finitos (FEMM) com dados experimentais obtidos em bancada.

Este projeto foi desenvolvido como material de apoio pedagógico e técnico para a disciplina **4302213 - Física Experimental III (IFUSP)** no primeiro semestre de **2026**.

---

## 📌 Contexto do Experimento
A atividade **Exp2Atv1** foca no estudo do movimento de elétrons sob a ação de campos elétricos. O objetivo fundamental é estabelecer uma ponte entre dois domínios:

1.  **Experimental:** Coleta de dados reais no TRC através da variação da tensão de aceleração ($V_{ac}$) e da tensão de deflexão aplicada às placas ($V_p$).
2.  **Simulado:** Modelagem da geometria real das placas utilizando o software **FEMM**. Esta abordagem permite extrair mapas de potencial e campo elétrico com alta fidelidade, capturando os **efeitos de borda** que são sistematicamente ignorados por modelos analíticos simplificados de placas infinitas.

---

## ⚛️ Fundamentação Teórica

### Equação de Laplace e Elementos Finitos
O potencial elétrico $V(x,y)$ na região entre as placas defletoras é determinado pela resolução da **Equação de Laplace**:

$$\nabla^2 V = \frac{\partial^2 V}{\partial x^2} + \frac{\partial^2 V}{\partial y^2} = 0$$

Neste projeto, a solução é obtida numericamente via **FEMM (Finite Element Method Magnetics)**, o que gera um mapa vetorial realista do campo elétrico local.

### Integração da Trajetória
A dinâmica do elétron no interior do tubo é governada pela **Força de Lorentz**:

$$m\frac{d\vec{v}}{dt} = q\vec{E}$$

O notebook processa essa dinâmica através de métodos de integração numérica (Euler/Verlet). Considerando que a energia cinética inicial é definida pela tensão de aceleração $V_{ac}$, o passo temporal $dt$ é condicionado pelo passo espacial $dx$ da seguinte forma:

$$dt = \frac{dx}{v_x} = dx \sqrt{\frac{m}{2qV_{ac}}}$$

---

## 🛠️ O Fluxo de Trabalho (Pipeline)

O projeto segue um pipeline reprodutível que conecta simulação física, extração de dados e análise numérica:

```mermaid
flowchart LR

A[FEMM - Simulação eletrostática] --> B[Lua Script - Extração de campo elétrico]
B --> C[CSV estruturado: x, y, Ex, Ey, V]
C --> D[Python - Interpolação do campo]
D --> E[Integração numérica: Euler / Verlet]
E --> F[Trajetória do elétron]
F --> G[Comparação experimental - dados de bancada]

style A fill:#d9f2d9,stroke:#2e7d32,stroke-width:1px
style B fill:#e3f2fd,stroke:#1565c0,stroke-width:1px
style C fill:#fff3e0,stroke:#ef6c00,stroke-width:1px
style D fill:#ede7f6,stroke:#5e35b1,stroke-width:1px
style E fill:#fce4ec,stroke:#c2185b,stroke-width:1px
style F fill:#e0f7fa,stroke:#00838f,stroke-width:1px
style G fill:#f3e5f5,stroke:#6a1b9a,stroke-width:1px
```

---

## 💻 Funcionalidades do Notebook

* **Interpolação de Campo:** Utilização da classe `RegularGridInterpolator` para reconstruir o campo elétrico em qualquer coordenada do espaço contínuo.
* **Integração Numérica:** Algoritmos de Euler e Verlet implementados para resolver as equações diferenciais de movimento.
* **Comparação Experimental:** Interface para inserção de dados de bancada ($h$ vs $V_p$) com geração automática de gráficos de sobreposição.
* **Análise de Sensibilidade:** Estudo da influência de diferentes valores de $V_{ac}$ na deflexão final do feixe na tela.

---

## 🌐 Visualização Online

A simulação pode ser acessada diretamente pelo site do projeto, com melhor experiência visual e navegação:

👉 **[Abrir visualização principal (GitHub Pages)](https://gutermanjunior.github.io/Lab3_2026-Exp2Atv1-CRT_Simulation_Electric_Field/)**


Caso ocorra algum problema de carregamento ou compatibilidade, utilize a versão alternativa via NBViewer:

🔁 **[Abrir notebook no NBViewer](https://nbviewer.org/github/gutermanjunior/Lab3_2026-Exp2Atv1-CRT_Simulation_Electric_Field/blob/main/Exp2Atv1%20simulacao_TRC%20Guterman.ipynb)**

---

## 📁 Estrutura do Repositório

```bash
├── figures/                               # Imagens utilizadas na visualização web (index.html)
├── CITATION.cff                           # Metadados para citação acadêmica
├── index.html                             # Versão renderizada para visualização web (GitHub Pages)
├── Exp2Atv1 simulacao_TRC Guterman.ipynb  # Notebook principal de análise
├── electric_field_data.csv                # Banco de dados de exemplo (saída do FEMM)
├── script.lua                             # Automação para extração de dados no FEMM
└── README.md                              # Documentação do projeto
```

---

## 🚀 Como Usar

1.  **Clone o repositório:**
    ```bash
    git clone [https://github.com/gutermanjunior/Lab3_2026-Exp2Atv1-CRT_Simulation_Electric_Field.git](https://github.com/gutermanjunior/Lab3_2026-Exp2Atv1-CRT_Simulation_Electric_Field.git)
    ```
2.  **Prepare o ambiente:**
    Instale as dependências necessárias:
    `pip install numpy matplotlib scipy pandas seaborn`
3.  **Gere os dados de campo:**
    Rode sua simulação no FEMM e execute o `script.lua` para criar o arquivo `electric_field_data.csv`.
4.  **Execute a análise:**
    Abra o notebook `Exp2Atv1_simulacao_TRC.ipynb` e carregue o CSV gerado.
5.  **Validação Experimental:**
    Utilize as células de dados experimentais para inserir seus valores coletados em laboratório ($V_p$ e $V_{ac}$) e comparar com a simulação.

---

## 📊 Exemplo de Resultado

> **Figura 1:** *Visualização da trajetória simulada. Parâmetros típicos: V<sub>ac</sub> = 1000 V e V<sub>p</sub> = 50 V. O desvio observado na tela do TRC é comparado ponto a ponto com a simulação numérica.*

---

## 🎯 Objetivo do Projeto
O propósito central desta ferramenta é superar as limitações dos modelos analíticos ideais. Ao incorporar os **efeitos de borda** e as geometrias reais das placas, o projeto busca uma concordância quantitativa rigorosa entre as previsões computacionais e as evidências experimentais.

---

## 📜 Como Citar
Se este código for útil para o seu relatório ou pesquisa, utilize o arquivo `CITATION.cff` disponível neste repositório ou a referência abaixo:

> **Araujo Junior, G. R.** (2026). *Simulação de Trajetória em Tubo de Raios Catódicos (TRC): Exp2Atv1*. GitHub Repository. `https://github.com/gutermanjunior/Lab3_2026-Exp2Atv1-CRT_Simulation_Electric_Field`

---

## 👨‍🔬 Créditos e Instituição

* **Desenvolvedor:** Guterman Rodrigues de Araujo Junior
* **Vínculo:** Mestrado em Física - IFUSP
* **Disciplina:** 4302213 - Física Experimental III
* **Local:** Instituto de Física da Universidade de São Paulo (IFUSP)

---
<div align="center">
  <sub>Construído com ❤️ para a comunidade acadêmica do IFUSP.</sub>
</div>
