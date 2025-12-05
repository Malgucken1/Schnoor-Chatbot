# 🤖 Schnoor's Agentic RAG Chatbot

Ein interaktiver Chatbot, der die **Retrieval-Augmented Generation (RAG)**-Architektur nutzt und durch einen **LangChain Agenten** erweitert wird. Der Chatbot kann Fragen basierend auf einer Vektor-Datenbank (Supabase) beantworten und verfügt über eine Funktion zur **Bildanalyse (Vision)** von hochgeladenen Dokumenten oder Plänen mittels GPT-4o.

## ✨ Features

* **Agentic RAG:** Intelligente Beantwortung von Fragen durch einen LangChain Agenten, der bei Bedarf ein spezialisiertes `retrieve`-Tool für datenbankgestützte Antworten nutzt.
* **Multi-Dokumenten-Support:** Hochladen und Verarbeiten von **PDF-, DOCX- und TXT-Dateien**.
* **GPT-4o Vision Integration:** Analyse und Text-Extraktion aus hochgeladenen **Bildern (PNG, JPG)**, ideal für die Verarbeitung von Raumplänen oder Diagrammen.
* **Chat-Historie und Management:** Speicherung und Verwaltung des Chat-Verlaufs pro Sitzung.
* **Sichere Authentifizierung:** Zugriffsschutz über ein einfaches Passwort-Login (konfiguriert über Streamlit Secrets).
* **Technologie-Stack:**
    * **LLM:** GPT-4o und GPT-4o-mini (für Vision)
    * **Vektor-Datenbank:** Supabase
    * **Frameworks:** Streamlit (UI), LangChain (Orchestrierung)

## 🛠️ Installation und Setup

### 1. Abhängigkeiten

Dieses Projekt erfordert Python 3.9+. Installiere die notwendigen Pakete:

```bash
pip install streamlit pypdf docx Pillow langchain_openai langchain_core langchain_community langchain supabase openai

2. API-Schlüssel & Secrets

Der Chatbot nutzt Streamlit Secrets, um sensible Daten sicher zu speichern. Erstelle eine Datei namens .streamlit/secrets.toml in deinem Projektverzeichnis mit folgendem Inhalt:
Ini, TOML

# .streamlit/secrets.toml

# Wichtig: Dies ist das Passwort für den Streamlit-Login.
APP_PASSWORD = "dein_sicheres_passwort" 

# OpenAI
OPENAI_API_KEY = "sk-..."

# Supabase (Für Vektor-Datenbank)
SUPABASE_URL = "[https://abc.supabase.co](https://abc.supabase.co)"
SUPABASE_SERVICE_KEY = "eyJ..." 

3. Supabase Vektor-Datenbank

Stelle sicher, dass deine Supabase-Datenbank die documents-Tabelle und die match_documents-Funktion für die Ähnlichkeitssuche eingerichtet hat, die mit den text-embedding-3-small Embeddings kompatibel sind.
🚀 Ausführung des Chatbots

Starte die Streamlit-Anwendung über dein Terminal im Projektverzeichnis:
Bash

streamlit run <dein_skriptname>.py

Verwendung

    Login: Gib das in secrets.toml definierte APP_PASSWORD ein.

    Datei-Upload (RAG-Erweiterung):

        Nutze die Sidebar, um PDF, DOCX, TXT oder Bilder hochzuladen.

        Bei Text-Dokumenten wird der Inhalt extrahiert und kann dem aktuellen Chat hinzugefügt werden.

        Bei Bildern (Pläne, Diagramme) wird die extract_text_from_image-Funktion aufgerufen, um eine Beschreibung und Analyse (Vision) zu erhalten, die als Kontext dient.

    Chatten: Stelle Fragen. Der Agent entscheidet, ob er das retrieve-Tool (RAG) für faktenbasierte Antworten oder seine allgemeine LLM-Wissenbasis nutzt.

    Chat-Verwaltung: Neue Chats können in der Sidebar gestartet und Titel automatisch generiert werden.

💡 Architektur-Highlights

Der Kern des Systems ist der LangChain AgentExecutor, der die Kontrolle über den Gesprächsverlauf und die Tool-Nutzung innehat.
Komponente	Zweck	Technologie
User Interface	Interaktive Benutzeroberfläche	Streamlit
Agent	Orchestriert den Workflow und entscheidet über Tool-Nutzung.	LangChain create_tool_calling_agent
Retrieve Tool	Führt die Vektor-Suche (RAG) in der Supabase-Datenbank durch.	LangChain vector_store.similarity_search
Vision	Extrahiert Text und analysiert hochgeladene Bilder.	OpenAI Client (gpt-4o-mini)
Datenbank	Speichert die Vektor-Embeddings der Dokumente.	Supabase Vector Store
