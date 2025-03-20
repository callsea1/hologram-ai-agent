# AI Hologram Agent

An interactive AI agent with a dynamic 3D hologram face that changes based on the agent's emotional state and conversation context. bloop

**Live Demo:** [https://callsea1.github.io/hologram-ai-agent/](https://callsea1.github.io/hologram-ai-agent/)

## Features

- Real-time 3D hologram face that responds to emotional context
- Natural language processing using OpenAI
- Dynamic facial expressions and emotions
- Modern web interface built with SvelteKit
- Real-time emotion analysis and response generation

## Tech Stack

- SvelteKit
- Three.js with Svelte-Three
- OpenAI API
- TailwindCSS
- TypeScript
- PostgreSQL (via Drizzle)

## Getting Started

1. Clone the repository:

```bash
git clone https://github.com/callsea1/hologram-ai-agent.git
cd hologram-ai-agent
```

2. Install dependencies:

```bash
npm install
```

3. Create a `.env` file in the root directory and add your OpenAI API key:

```
OPENAI_API_KEY=your_api_key_here
```

4. Start the development server:

```bash
npm run dev
```

5. Open your browser and navigate to `http://localhost:5173`

## Development

- `npm run dev` - Start development server
- `npm run build` - Build for production
- `npm run preview` - Preview production build
- `npm run test` - Run tests
- `npm run storybook` - Start Storybook

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

This project is licensed under the MIT License - see the LICENSE file for details.

# sv

Everything you need to build a Svelte project, powered by [`sv`](https://github.com/sveltejs/cli).

## Creating a project

If you're seeing this, you've probably already done this step. Congrats!

```bash
# create a new project in the current directory
npx sv create

# create a new project in my-app
npx sv create my-app
```

## Developing

Once you've created a project and installed dependencies with `npm install` (or `pnpm install` or `yarn`), start a development server:

```bash
npm run dev

# or start the server and open the app in a new browser tab
npm run dev -- --open
```

## Building

To create a production version of your app:

```bash
npm run build
```

You can preview the production build with `npm run preview`.

> To deploy your app, you may need to install an [adapter](https://svelte.dev/docs/kit/adapters) for your target environment.
