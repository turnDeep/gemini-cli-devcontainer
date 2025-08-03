# Use Node.js 20 as base image
FROM mcr.microsoft.com/devcontainers/javascript-node:1-20-bookworm

# Install additional dependencies
RUN apt-get update && apt-get install -y \
    curl \
    wget \
    jq \
    python3 \
    python3-pip \
    build-essential \
    && rm -rf /var/lib/apt/lists/*

# Switch to node user
USER node

# Set up workspace directory
WORKDIR /workspace

# Create necessary directories for Gemini CLI
RUN mkdir -p /home/node/.gemini \
    && mkdir -p /home/node/.gemini/tmp \
    && mkdir -p /workspace/.gemini

# Install Gemini CLI globally
RUN npm install -g @google/gemini-cli

# Create a startup script for easier authentication
RUN echo '#!/bin/bash\n\
echo "🚀 Gemini CLI Development Container Ready!"\n\
echo ""\n\
echo "📝 Quick Start Guide:"\n\
echo "-------------------"\n\
echo ""\n\
echo "Option 1: Browser Authentication (Recommended for first-time setup)"\n\
echo "  1. Run: gemini"\n\
echo "  2. Select your preferred theme"\n\
echo "  3. Choose \"Login with Google\""\n\
echo "  4. Open the provided URL in your browser"\n\
echo "  5. Complete Google sign-in"\n\
echo ""\n\
echo "Option 2: API Key Authentication"\n\
echo "  1. Get API key from: https://aistudio.google.com/app/apikey"\n\
echo "  2. Set environment variable:"\n\
echo "     export GEMINI_API_KEY=\"your-api-key-here\""\n\
echo "  3. Or create .env file with: GEMINI_API_KEY=your-api-key-here"\n\
echo "  4. Run: gemini"\n\
echo "  5. Choose \"Gemini API Key\" when prompted"\n\
echo ""\n\
echo "📌 Useful Commands:"\n\
echo "  gemini --help     : Show all available options"\n\
echo "  gemini --version  : Check installed version"\n\
echo "  /auth            : Change authentication method"\n\
echo "  /help            : Show commands within Gemini CLI"\n\
echo ""\n\
echo "🔗 Resources:"\n\
echo "  - GitHub: https://github.com/google-gemini/gemini-cli"\n\
echo "  - API Keys: https://aistudio.google.com/app/apikey"\n\
echo "  - Documentation: https://ai.google.dev/gemini-api/docs"\n\
echo ""\n\
' > /home/node/gemini-setup.sh && chmod +x /home/node/gemini-setup.sh

# Set shell to zsh
SHELL ["/bin/zsh", "-c"]

# Expose ports for OAuth callbacks
EXPOSE 3000 3002 5000 8080

# Set the entrypoint
ENTRYPOINT ["/bin/zsh"]