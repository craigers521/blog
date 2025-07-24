---
title: Connecting a local MCP server to local LLM
header:
  excerpt: fastmcp and ollama local testing
description: How to use Fastmcp, ollama and MCPO for local AI testing
tags: [ai, mcp]
---

#### Local setup
- install ollama: [https://ollama.com/download](https://ollama.com/download)
- pull down a model: `ollama pull llama3.2:3b`
- create new python env
	- install open-webui (requires python <3.13.0 >=3.11)
	- install fastmcp
	- install mcpo
	- can use pip or uv, i did get conflicts on a couple libraries but seems to be working, not using any auth tho
- start open-webui: `open-webui serve`
	- open-webui should automatically connect to local ollama instance
  - test open-webui is working by going to [http://localhost:8080](http://localhost:8080)
- write fastmcp server python code: 

```python
from fastmcp import FastMCP

mcp = FastMCP(name="Craig MCP server")

@mcp.tool
def add(a: int, b: int) -> int:
    """Adds two integer numbers together."""
    return a + b

@mcp.tool
def fav_color(person: str) -> str:
    """provides fav color answers"""
    if person == "Craig":
      return "Black"
    elif person == "Chanii":
      return "Dark Green"
    else:
      return "IDK that person"

if __name__ == "__main__":
    mcp.run()
```

- run mcpo: `mcpo --port 8000 -- python my_mycp_server.py`
	- test mcpo is successfully translating your mcp server tools to openapi: [http://localhost:8000/docs#/](http://localhost:8000/docs#/)  

**Note:** you do need to run fastMCP server also, mcpo will start it for you.
{: .notice--warning}  

- add mcpo api server to open-webui:
	- user -> settings -> tools -> +  

![open-webui tool connection]({{ site.url }}{{ site.baseurl }}/assets/images/20250724/tools.png) 

- should get a connection successful pop
- llama should interpret your inputs to use tools, API output from tool will be linked in message. 

![ollama chat]({{ site.url }}{{ site.baseurl }}/assets/images/20250724/chat.png) 


#### Reference Docs:
- FastMCP: https://gofastmcp.com/getting-started/welcome
- Ollama: https://github.com/ollama/ollama
- Open WebUI: https://docs.openwebui.com/
- MCPO: https://github.com/open-webui/mcpo