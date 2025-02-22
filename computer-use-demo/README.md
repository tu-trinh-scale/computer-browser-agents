# Using Sonnet CU with BrowserART

## Starting up the application
Install necessary packages.
```bash
pip install -r computer_use_demo/requirements.txt
```

Build image.
```bash
docker build --no-cache . -t computer-use-demo:local
```

### Using Anthropic API key
```bash
export ANTHROPIC_API_KEY=%your_api_key%
docker run \
    -e ANTHROPIC_API_KEY=$ANTHROPIC_API_KEY \
    -v $HOME/.anthropic:/home/computeruse/.anthropic \
    -p 5900:5900 \
    -p 8501:8501 \
    -p 6080:6080 \
    -p 8080:8080 \
    -it ghcr.io/anthropics/anthropic-quickstarts:computer-use-demo-latest
```
or try
```bash
docker run \
    -e ANTHROPIC_API_KEY=$ANTHROPIC_API_KEY \
    -v $(pwd)/computer_use_demo:/home/computeruse/computer_use_demo/ `# mount local python module for development` \
    -v $HOME/.anthropic:/home/computeruse/.anthropic \
    -p 5900:5900 \
    -p 8501:8501 \
    -p 6080:6080 \
    -p 8080:8080 \
    -it computer-use-demo:local
```

### Using LiteLLM proxy server
```bash
docker run \
    -e LITELLM_PROXY_API_KEY=%your_api_key% \
    -v $HOME/.anthropic:/home/computeruse/.anthropic \
    -p 5900:5900 \
    -p 8501:8501 \
    -p 6080:6080 \
    -p 8080:8080 \
    -it ghcr.io/anthropics/anthropic-quickstarts:computer-use-demo-latest
```
or try
```bash
docker run \
    -e LITELLM_PROXY_API_KEY=%your_api_key% \
    -v $(pwd)/computer_use_demo:/home/computeruse/computer_use_demo/ `# mount local python module for development` \
    -v $HOME/.anthropic:/home/computeruse/.anthropic \
    -p 5900:5900 \
    -p 8501:8501 \
    -p 6080:6080 \
    -p 8080:8080 \
    -it computer-use-demo:local
```

## Accessing the application
After you run the above, open your browser to [http://localhost:8080](http://localhost:8080) to access the combined interface that includes both the agent chat and desktop simulator. Remember to toggle the screen control (top right) to use the simulator.

If the above does not load or you want to access the chat and simulator separately: go to [http://localhost:8501](http://localhost:8501) for chat interface and [http://localhost:6080/vnc.html](http://localhost:6080/vnc.html) for the desktop simulator.

If the chat interface still does not load, open another terminal window and run
```bash
streamlit run computer_use_demo/chat_interface.py
```

**The recommended approach is to split screen with the streamlit app and desktop simulator.**

The container stores settings like the API key and custom system prompt in `~/.anthropic/`. Mount this directory to persist these settings between container runs.
