# AI Orchestration Lab

Lab:

https://github.com/Azure/intro-to-intelligent-apps/tree/main/labs/03-orchestration

## Getting Started

If you are...

- A Python developer (req. `python` and [windows only] VS Build Tools or C/C++ workload for Visual Studio to install `chromadb` dependecy)
    1. Go to the previous link and clone onto your local machine.
    ```
    git clone https://github.com/Azure/intro-to-intelligent-apps.git
    ```
    2. Create a virtual environement and activate it.
    ```
    python -m venv env
    # on windows
    ./env/Scripts/Activate.ps1
    # on unix
    source env/bin/activate
    ```
    3. Install dependencies
    ```
    pip install -r requirements.txt
    ```
    4. Install `ipykernel` if needed by your notebook runner (or use your preferred one) 
    ```
    pip install ipykernel
    ```
    5. Create a new file `.env` from `.env.example`
    6. Go to `/labs/03-orchestration` (or start from `02` if you have time) and start going through the notebooks using the Visual Studio Code Extension or your preferred noteboook runner.

- A data person (req. access to any kind of notebook development environment)
    1. Go to your preferred jupyter notebook environment and clone/open the notebooks in `/labs/03-orchestration` from the link.
    2. Install dependencies
    ```
    pip install -r requirements.txt
    ```
    3. Create a new file `.env` from `.env.example`
    4. Go to `/labs/03-orchestration` (or start from `02` if you have time) and start going through the notebooks.

- A Docker person (req. docker on your machine)
    1. Go to the previous link and clone onto your local machine.
    ```
    git clone https://github.com/Azure/intro-to-intelligent-apps.git
    ```
    2. Add a file `/images/intro-to-intelligent-apps/compose.yml` with the following contents
    ```
    services:
    devmachine:
        build:
        context: ../..
        dockerfile: images/intro-to-intelligent-apps/Dockerfile
        volumes:
        - ../../labs:/usr/src/app/labs
        ports:
        - 8000:8888
        env_file: ../../.env
        command: jupyter lab --allow-root --ip 0.0.0.0
    ```
    3. Add a file `/images/intro-to-intelligent-apps/Dockerfile` with the following contents
    ```
    FROM python:3.11

    WORKDIR /usr/src/app

    RUN pip install jupyter

    COPY requirements.txt ./

    RUN pip install --no-cache-dir -r requirements.txt

    COPY labs ./labs/
    COPY .env.example ./env.example
    ```
    4. Create a new file `.env` from `.env.example`
    5. Build the image
    ```
    cd images/intro-to-intelligent-apps
    docker-compose build
    ```
    6. Start the image
    ```
    docker-compose up
    ```
    7. Access the Jupyer Lab on port `8000` configured in `compose.yml` using the link/token output in the terminal.
    8. Go to `/labs/03-orchestration` (or start from `02` if you have time) and start going through the notebooks.

Extra: Installing docker

- On Windows
    ```
    choco install docker-desktop
    ```
- On MacOS
    ```
    brew install docker
    ```
- On Linux (see also https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository)
    ```
    # Add Docker's official GPG key:
    sudo apt-get update
    sudo apt-get install ca-certificates curl gnupg
    sudo install -m 0755 -d /etc/apt/keyrings
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
    sudo chmod a+r /etc/apt/keyrings/docker.gpg

    # Add the repository to Apt sources:
    echo \
    "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
    $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
    sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
    sudo apt-get update

    # Install the Docker packages.
    sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
    ```

### Other Links

API Reference

- https://learn.microsoft.com/en-us/azure/ai-services/openai/reference

Resources

- Embeddings
    - Word2Vec https://arxiv.org/abs/1301.3781
    - Img2Vec https://arxiv.org/abs/2304.12535
    - https://openai.com/blog/new-and-improved-embedding-model
- Orchestration Frameworks
    - https://www.langchain.com
    - https://github.com/deepset-ai/haystack
    - https://github.com/microsoft/semantic-kernel

Examples

- https://github.com/RedisVentures/redis-product-search
    - https://docsearch.redisventures.com
    - https://ecommerce.redisventures.com
    - https://github.com/UKPLab/sentence-transformers
    - https://github.com/christiansafka/img2vec
- https://github.com/ruoccofabrizio/azure-open-ai-embeddings-qna
- https://github.com/Azure-Samples/azure-search-openai-demo
