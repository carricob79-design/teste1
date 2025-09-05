#!/bin/bash
# Script completo para instalar e rodar Stralive no ambiente do desenvolvedor

ZIP_PATH="$HOME/stralive_starter.zip"
INSTALL_DIR="$HOME/stralive_starter"

echo "=== Limpando instalações antigas ==="
rm -rf "$INSTALL_DIR"

echo "=== Verificando se o ZIP existe ==="
if [ ! -f "$ZIP_PATH" ]; then
    echo "ERRO: ZIP do Stralive não encontrado em $ZIP_PATH"
    exit 1
fi

echo "=== Descompactando o ZIP ==="
unzip "$ZIP_PATH" -d "$INSTALL_DIR"

# Backend
BACKEND_DIR="$INSTALL_DIR/backend"
if [ ! -f "$BACKEND_DIR/package.json" ]; then
    echo "ERRO: package.json do backend não encontrado!"
    exit 1
fi

echo "=== Instalando dependências do backend ==="
cd "$BACKEND_DIR"
npm install

# Rodar backend em segundo plano
echo "=== INICIANDO BACKEND em segundo plano ==="
nohup npm start > "$BACKEND_DIR/backend.log" 2>&1 &

# Frontend
FRONTEND_DIR="$INSTALL_DIR/frontend"
if [ ! -f "$FRONTEND_DIR/package.json" ]; then
    echo "ERRO: package.json do frontend não encontrado!"
    exit 1
fi

echo "=== Instalando dependências do frontend ==="
cd "$FRONTEND_DIR"
npm install

echo "=== Stralive pronto ==="
echo "Backend rodando em background. Logs: $BACKEND_DIR/backend.log"
echo "Para rodar o frontend, execute:"
echo "cd $FRONTEND_DIR && npx expo start --tunnel"
