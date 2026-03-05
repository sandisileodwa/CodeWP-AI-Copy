BUILD CodeWP AI - WordPress AI Code Generator 

## **1. Product Overview**
This document outlines the requirements for **CodeWP AI**, an AI-powered code generation platform built specifically for WordPress creators. Inspired by the core concept of Telex (an AI-assisted environment for WordPress blocks), this product will extend the functionality to be a comprehensive, context-aware assistant for generating accurate code snippets across the entire WordPress ecosystem, including core PHP, JavaScript, WooCommerce, and major plugins.

**Core Vision:** To drastically reduce development time and lower the technical barrier for WordPress creators by providing an intelligent, specialized tool that understands WordPress context, best practices, and common patterns.

**Target Users:**
*   WordPress Developers (Freelance & Agency)
*   WordPress Site Builders
*   WooCommerce Store Managers
*   WordPress Plugin & Theme Authors

## **2. Core Features & User Stories**
**2.1. Primary Features:**
1.  **Context-Aware Code Snippet Generation:** Generate functional code snippets in PHP and JavaScript based on natural language prompts (e.g., "Create a shortcode to display recent posts from category X").
2.  **WordPress-Specific Specialization:** The AI model is fine-tuned on WordPress Core, popular plugin (WooCommerce, ACF, Gravity Forms) APIs, and Theme development standards.
3.  **Block Editor (Gutenberg) Integration:** Directly assist in creating custom dynamic blocks, block variations, and editor plugins, as per the Telex paradigm.
4.  **WooCommerce Code Generator:** Generate snippets for customizing product pages, cart functions, checkout fields, and payment gateways.
5.  **Plugin & Theme Boilerplate Generator:** Scaffold starter code for custom plugins or child themes based on user specifications.
6.  **Code Explanation & Documentation:** Explain existing WordPress code snippets in plain language.
7.  **Security & Best Practices:** Inline code reviews focusing on WordPress security (data validation, sanitization, escaping, nonces) and performance (caching, optimized queries).

**2.2. User Stories:**
*   As a site builder, I want to describe a functionality so I can get a ready-to-use, secure PHP code snippet to paste into my theme's `functions.php`.
*   As a WooCommerce developer, I want to generate a snippet to add a custom field to the product page and save it to the order, so I don't have to search through documentation.
*   As a block developer, I want to use an AI-assisted environment to quickly scaffold the `block.json`, edit.js, and render.php for a new custom block.

## **3. Detailed Specifications**
**3.1. Input/Output Specification:**
*   **Input:** Natural language prompt, target context (Plugin, Theme, `functions.php`, Block), and selected WordPress/Plugin version.
*   **Output:**
    *   A clean, well-commented code snippet in the target language.
    *   A brief explanation of the code.
    *   Usage instructions (e.g., where to place the code).
    *   Related `use` cases or warnings.

**3.2. Accuracy & Performance Metrics:**
*   **Code Correctness:** >95% of generated snippets should be syntactically correct and follow WordPress Coding Standards.
*   **Functional Accuracy:** >85% of snippets should perform the intended task without logical errors.
*   **Response Time:** Average generation time < 5 seconds for snippets under 50 lines.

## **4. Technical Architecture**
**4.1. System Architecture (High-Level):**
A serverless microservices architecture is recommended for scalability.
```
[User Interface: React App] -> [API Gateway] -> [Orchestrator Service]
                                                          |
                                    -> [AI Inference Service (LLM)] -> [Post-Processor]
                                    -> [Context Service (WP Docs, Codex)]
                                    -> [User Session & History DB]
```
**4.2. Core Components:**
1.  **Orchestrator:** Parses the user request, enriches it with context from the Context Service, and calls the AI Inference Service.
2.  **AI Inference Service:** Hosts the fine-tuned language model. Handles the core prompt engineering and generation.
3.  **Context Service:** Manages a searchable knowledge base of WordPress Core documentation, Plugin APIs, and official code examples to provide real-time context to the AI.
4.  **Post-Processor:** Formats raw AI output, adds standard code comments, and runs basic security/standard checks.

## **5. System Structure (Project Layout)**
```
codewp-ai/
├── backend/
│   ├── orchestrator/          # Main request handler
│   ├── inference-service/     # LLM integration & management
│   ├── context-service/       # Documentation vector DB interface
│   └── shared/                # Common utilities, DTOs
├── frontend/
│   ├── src/
│   │   ├── components/        # UI components (PromptInput, CodeDisplay)
│   │   ├── contexts/          # React contexts (Theme, Auth)
│   │   └── pages/             # Main application views
│   └── public/
├── ml/
│   ├── training/              # Scripts for fine-tuning the model
│   ├── datasets/              # Curated WordPress code datasets
│   └── evaluation/            # Code for testing model output quality
└── infrastructure/
    ├── terraform/             # IaC for cloud provisioning
    └── docker/                # Container configurations
```

## **6. Technology Stack (Techstack)**
*   **Backend (Orchestrator/Context Service):** Node.js (Express/NestJS) or Python (FastAPI). **Python is preferred for ML integration**.
*   **AI/ML Core:**
    *   **Base Model:** A large code-specialized model (e.g., CodeLlama, StarCoder).
    *   **Framework:** Hugging Face `transformers`, LangChain for orchestration, LlamaIndex for context retrieval.
    *   **Fine-tuning:** PEFT/LoRA techniques for efficient domain adaptation.
*   **Frontend:** React.js (TypeScript) with a code-oriented UI library (e.g., CodeMirror/Monaco Editor for code display).
*   **Context Database:** Vector database (Pinecone, Weaviate, or pgvector) for storing and retrieving documentation embeddings.
*   **Primary Database:** PostgreSQL for user data, snippet history, and application state.
*   **Infrastructure:** Docker, Kubernetes (or managed container service), deployed on AWS/GCP/Azure.

## **7. Boilerplate Code Example**
**Example: Generated `functions.php` Snippet**
```php
<?php
/**
 * Adds a custom welcome message for logged-in users in the site header.
 * Generated by CodeWP AI.
 *
 * @package YourThemeName
 */

/**
 * Displays a personalized welcome message.
 * Hook this function to `wp_head` or a theme hook.
 *
 * @return void
 */
function codewp_display_user_welcome() {
    if ( is_user_logged_in() ) {
        $current_user = wp_get_current_user();
        // Sanitize user display name for safe output
        $user_name = esc_html( $current_user->display_name );
        echo '<div class="user-welcome">Welcome back, ' . $user_name . '!</div>';
    }
}
// Uncomment and adjust the hook as needed:
// add_action( 'wp_body_open', 'codewp_display_user_welcome' );
```

## **8. AI Cursor & Prompt Engineering Rules**
*   **System Prompt Foundation:** "You are a senior WordPress developer assistant. Always output secure, efficient, and standards-compliant code. Adhere to WordPress PHP and JavaScript coding standards."
*   **Context Injection:** Prepend every user prompt with relevant context: `[WordPress Version: 6.5][Context: Theme functions.php][Task: Create a shortcode]`.
*   **Structured Output:** Enforce a strict JSON output format from the AI before post-processing: `{"code": "...", "explanation": "...", "usage_hint": "..."}`.
*   **Security First:** The model must be instructed to **always** use core WordPress helper functions (`esc_html`, `wp_kses`, `sanitize_text_field`, `wp_nonce_field`) over raw PHP equivalents.

## **9. Data Schema (Key Entities)**
```sql
-- Core snippet generation history
CREATE TABLE snippets (
    id UUID PRIMARY KEY,
    user_id UUID REFERENCES users(id),
    prompt TEXT NOT NULL,
    generated_code TEXT NOT NULL,
    language VARCHAR(10), -- 'php', 'js'
    context VARCHAR(50), -- 'woocommerce', 'blocks', 'general'
    created_at TIMESTAMPTZ DEFAULT NOW()
);

-- Cached context embeddings from documentation
CREATE TABLE doc_embeddings (
    id UUID PRIMARY KEY,
    source VARCHAR(100), -- 'wp_core', 'woocommerce_docs'
    content TEXT NOT NULL,
    embedding VECTOR(1536), -- Vector size depends on model
    metadata JSONB
);
```

## **10. README.md (Project Start Guide)**
```markdown
# CodeWP AI - Development Setup

## Prerequisites
- Node.js >= 18, Python >= 3.10, Docker & Docker Compose
- Access to a hosted LLM API (OpenAI, Anthropic) or local Ollama instance

## Local Development
1.  Clone the repo: `git clone https://github.com/yourorg/codewp-ai.git`
2.  Copy environment variables: `cp .env.example .env`
3.  Configure your `.env` file (see below).
4.  Start services: `docker-compose up -d`
5.  Navigate to `http://localhost:3000` for the frontend.

## First-Time Model Setup
Run the context ingestion script to populate the vector database with WordPress documentation:
`python ml/scripts/ingest_docs.py --source wp_docs`
```

## **11. Environment Variables (.env.example)**
```env
# Backend Service
NODE_ENV=development
PORT=3001
SESSION_SECRET=your_session_secret_here

# Database
POSTGRES_HOST=localhost
POSTGRES_DB=codewp_ai
POSTGRES_USER=admin
POSTGRES_PASSWORD=strong_password_here

# Vector Database (for Context Service)
PINECONE_API_KEY=your_pinecone_key
PINECONE_INDEX_NAME=codewp-context-index

# AI Model Provider (Choose one)
# Option 1: OpenAI
OPENAI_API_KEY=your_openai_key
OPENAI_MODEL=gpt-4-turbo-preview

# Option 2: Local Ollama (for development)
OLLAMA_BASE_URL=http://localhost:11434
OLLAMA_MODEL=code-llama

# Frontend
VITE_API_BASE_URL=http://localhost:3001/api
```

---

This PRD provides a foundational blueprint for developing a **CodeWP AI**-like system, anchored by the **AI-assisted WordPress block development** concept from the Telex project and expanded into a full-stack, specialized code generation platform. The next steps would involve detailed sprint planning for each component, starting with the dataset curation and model fine-tuning pipeline.
