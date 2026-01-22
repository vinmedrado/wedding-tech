# wedding-tech
**Projeto pessoal:** envio automatizado de convites usando Google Forms, Python, Excel, Docker e Evolution API

---

# Projeto de Convites Automatizados
 
**Projeto pessoal:** envio automatizado de convites usando **Google Forms**, **Python**, **Excel**, **Docker** e **Evolution API**.

---

## Quando o casamento encontra a tecnologia

Para o meu próprio casamento, criei uma solução personalizada para enviar convites, organizar confirmações de presença e gerenciar transporte, tudo de forma **automatizada**.  
Neste exemplo, usamos **Excel** e **Power Query** para simular o fluxo.

---

## Estrutura do projeto

**wedding-tech/**  
├─ **README.md** # Este arquivo  
├─ **planilhas/**  
│  └─ **TABELA_EXEMPLO.xlsx** # Dados fictícios para teste seguro  
├─ **scripts/**  
│  └─ **envio_convites.py** # Script Python para envio automatizado  
├─ **docker-compose.yml** # Configuração do Docker  
└─ **.env.example** # Exemplo de arquivo .env

---

## Aba de exemplo – TABELA_EXEMPLO

A aba `TABELA_EXEMPLO` contém **dados fictícios** para simular o fluxo real:

| Carimbo de data/hora | Família / Grupo | Nome     | Você irá de Ônibus ? |
|--------------------|----------------|---------|---------------------|
| 01/01/2026 10:00   | Família Silva  | Vinicius | Sim                 |
| 01/01/2026 10:05   | Família Souza  | Maria    | Não                 |

---

## Power Query (Exemplo Seguro)

A query lê a aba `TABELA_EXEMPLO` e organiza os dados:

```m
Fonte = Excel.CurrentWorkbook(){[Name="TABELA_EXEMPLO"]}[Content]
TipoAlterado = Table.TransformColumnTypes(Fonte, {
    {"Carimbo de data/hora", type text}, 
    {"Família / Grupo", type text}, 
    {"Nome", type text}, 
    {"Você irá de Ônibus ?", type text}
})
in TipoAlterado

Observação: Para uso real, substitua a fonte por um Google Form ou Google Sheets publicado como CSV.
```
---

## Links de Formulário e PDF

Google Forms
No Excel, você pode gerar links dinâmicos para cada convidado:
=HIPERLINK(
 "https://docs.google.com/forms/d/e/EXEMPLO_DO_FORMULARIO/viewform?usp=pp_url"&
 "&entry.47010176="&SUBSTITUIR(A2;" ";"%20")&
 "&entry.163752669="&SUBSTITUIR(D2;" ";"%20"),
 "Confirmar Presença"
)

Google Drive (PDF)
Para anexar PDFs de convite, use links de download direto:
https://drive.google.com/uc?export=download&id=EXEMPLO_DO_PDF

Substitua EXEMPLO_DO_PDF pelo ID real do arquivo no Google Drive.

---

## Script Python (scripts/envio_convites.py)

O script faz a leitura da planilha de convidados, gera links dinâmicos para formulário e PDF, agrupa convidados por família, e envia mensagens e PDFs via Evolution API conectada pelo Docker.

Usa o arquivo .env para manter a API_KEY segura.

Exemplo de configuração .env:
API_KEY=SUACHAVEAQUI

No script Python, use:
from dotenv import load_dotenv
import os

load_dotenv()
API_KEY = os.getenv("API_KEY")


---

## Docker + Evolution API

O Docker Compose inicia a instância da Evolution API para envio de WhatsApp.
O script Python se conecta à API usando a API_KEY do .env.


---

## Fluxo do projeto (Visual)
```m
💌 Google Forms → coleta de respostas dos convidados (nome, família, transporte)
⬇️
📊 Planilhas + Power Query → organiza os dados
⬇️
🐍 Script Python → gera links, agrupa por família e prepara envio
⬇️
🐳 Docker + Evolution API → integração com WhatsApp
⬇️
💬 WhatsApp → envio de mensagens e PDFs
⬇️
🎉 Convidados → recebem o convite e confirmam presença
```
