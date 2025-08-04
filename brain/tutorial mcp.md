# Tutorial básico de MCP Server em Python

O Model Context Protocol (MCP) é um protocolo aberto que permite que aplicações de IA (como Claude ou Cursor) se conectem a ferramentas externas e fontes de dados de forma padronizada. Com MCP, você pode criar servidores que expõem funções ("tools") que podem ser chamadas por clientes MCP (como um chat com IA).

## Exemplo de MCP Server em Python

1. **Pré-requisitos:**
   - Python 3.7+
   - Instale o SDK MCP: `pip install mcp`

2. **Código básico de um MCP Server:**
```python
from mcp.server.fastmcp import FastMCP
import sqlite3

mcp = FastMCP("Community Chatters")

@mcp.tool()
def get_top_chatters():
    """Retorna os top chatters de um banco SQLite."""
    conn = sqlite3.connect('community.db')
    cursor = conn.cursor()
    cursor.execute("SELECT name, messages FROM chatters ORDER BY messages DESC")
    results = cursor.fetchall()
    conn.close()
    return [{"name": name, "messages": messages} for name, messages in results]

if __name__ == '__main__':
    mcp.run()
```

3. **Rodando o servidor:**
```bash
python server.py
```

4. **Integrando com Cursor ou Claude Desktop:**
- Adicione o servidor MCP nas configurações do Cursor ou Claude Desktop, apontando para o script Python.

## Links úteis
- Tutorial completo: https://www.digitalocean.com/community/tutorials/mcp-server-python
- Outro exemplo: https://mostafawael.medium.com/demystifying-the-model-context-protocol-mcp-with-python-a-beginners-guide-0b8cb3fa8ced

Este é um ponto de partida para criar e testar seu próprio MCP Server em Python!
