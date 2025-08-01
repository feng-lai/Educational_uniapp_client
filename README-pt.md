
[English](README.md)  
[日本語](README-jp.md)  
[العربية](README-ar.md)  
[Español](README-es.md)  

# 🎓 Aplicativo Educacional UniApp

Este é um aplicativo móvel para uma plataforma educacional desenvolvido com **UniApp**, um framework frontend multiplataforma baseado em Vue.js. Ele oferece uma experiência fluida para estudantes e professores acessarem conteúdos, gerenciarem cursos e interagirem com os materiais de aprendizagem.

> 🔗 Utilizado com uma API backend (não incluída neste repositório), este cliente serve como o portal móvel voltado aos estudantes.

## 📱 Funcionalidades

- 📚 **Lista de Cursos e Inscrição**
  - Navegar por cursos disponíveis
  - Inscrever-se ou sair de cursos
  - Páginas detalhadas dos cursos com vídeos e materiais

- 📝 **Tarefas e Deveres**
  - Visualizar tarefas atribuídas
  - Enviar respostas
  - Ver feedback e notas dos professores

- 🧪 **Provas e Questionários**
  - Questões objetivas e discursivas
  - Correção e feedback em tempo real

- 🗓 **Progresso de Aprendizagem**
  - Painel pessoal com tarefas concluídas e pendentes
  - Estatísticas de conclusão de cursos

- 💬 **Mensagens e Notificações**
  - Receber lembretes sobre prazos
  - Módulo de comunicação entre professor e aluno (se habilitado)

- 👤 **Centro do Usuário**
  - Editar perfil, alterar senha
  - Ver registros acadêmicos e relatórios de desempenho

## 🛠️ Tecnologias Utilizadas

- **Framework:** UniApp (baseado em Vue.js)  
- **UI Library:** uView / componentes personalizados  
- **Roteamento:** Roteamento nativo do UniApp  
- **Armazenamento:** Vuex + localStorage  
- **Plataformas:** H5, Mini Program WeChat, Android/iOS via HBuilderX

## 🚀 Primeiros Passos

### Pré-requisitos

- [HBuilderX](https://www.dcloud.io/hbuilderx.html) (IDE recomendada para UniApp)
- Node.js e npm (caso use compilação via CLI)
- API Backend (não incluída)

### Instalação

1. Clone o projeto:

```bash
git clone https://github.com/feng-lai/Educational_uniapp_client.git
cd Educational_uniapp_client
````

2. Abra o projeto no **HBuilderX** ou em um editor compatível com Vue.

3. Configure os endpoints da API:

   * Atualize a URL base da API em `common/request.js` ou outro arquivo de configuração.

4. Execute o app na plataforma desejada:

   * HBuilderX: Clique em "Executar no Navegador / Mini Programa / Emulador"
   * CLI: `npm run dev:%platform%` (ex: `dev:h5`, `dev:mp-weixin`)

## 📁 Estrutura do Projeto

```
Educational_uniapp_client/
├── pages/              # Telas principais
├── components/         # Componentes reutilizáveis
├── store/              # Vuex para gerenciamento de estado
├── static/             # Recursos estáticos (imagens, ícones)
├── common/             # Funções utilitárias, configuração de requisições
├── uni.scss            # Estilos globais
└── main.js             # Arquivo principal
```

## ⚙️ Dicas de Configuração

* ✅ Certifique-se de que a API suporta CORS para testes em H5
* 🔒 Utilize autenticação baseada em token (JWT é recomendado)
* 🌐 Suporte multilíngue pode ser adicionado com plugins de i18n

## 📄 Licença

Este projeto está licenciado sob a Licença MIT. Você pode modificá-lo e usá-lo em projetos pessoais ou comerciais com atribuição.

## 🙋 Autor

Desenvolvido e mantido por [feng-lai](https://github.com/feng-lai)

---

> 📢 *Pull requests e feedbacks são bem-vindos! Se você estiver desenvolvendo um app educacional, este projeto pode ser um ótimo ponto de partida para seu cliente móvel.*

