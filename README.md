# 📊 Sistema de Aquisição de Dados Inerciais com MPU6050 e Cartão SD

## 📑 Sumário
- [🎯 Objetivos](#-objetivos)
- [📋 Descrição do Projeto](#-descrição-do-projeto)
- [⚙️ Funcionalidades Implementadas](#️-funcionalidades-implementadas)
- [🛠️ Requisitos do Projeto](#️-requisitos-do-projeto)
- [📂 Estrutura do Código](#-estrutura-do-código)
- [🖥️ Como Compilar](#️-como-compilar)
- [🤝 Contribuições](#-contribuições)
- [📽️ Demonstração em Vídeo](#️-demonstração-em-vídeo)
- [💡 Considerações Finais](#-considerações-finais)

## 🎯 Objetivos
- Desenvolver um sistema para aquisição e registro de dados de movimento (aceleração e giroscópio) utilizando o sensor MPU6050.
- Implementar algoritmos de filtragem (Filtro de Kalman) para obter dados de orientação mais precisos.
- Realizar o armazenamento contínuo dos dados sensoriais em um cartão SD.
- Exibir informações em tempo real em um display OLED.
- Fornecer um script Python para visualização e análise dos dados coletados.

## 📋 Descrição do Projeto
Este projeto consiste em um sistema de aquisição de dados inerciais (IMU) baseado no microcontrolador Raspberry Pi Pico, utilizando o sensor MPU6050 para capturar informações de aceleração e velocidade angular. Os dados brutos são processados por um Filtro de Kalman para estimar ângulos de inclinação e arfagem com maior precisão. As leituras, tanto brutas quanto processadas, são armazenadas em um cartão SD para análise posterior. Além disso, um display OLED SSD1306 é integrado para visualização em tempo real de informações importantes.

O sistema é ideal para aplicações que requerem o monitoramento e registro de movimento, como análise de vibração, rastreamento de orientação, robótica móvel, ou experimentos de física.

## ⚙️ Funcionalidades Implementadas
1.  **Aquisição de Dados do MPU6050:**
    * Leitura de dados brutos do acelerômetro (eixos X, Y, Z) e do giroscópio (eixos X, Y, Z).
    * Inicialização e configuração do sensor MPU6050 via comunicação I2C.
    * Conversão dos valores brutos em unidades físicas (g para aceleração, graus/s para velocidade angular).

2.  **Processamento de Dados com Filtro de Kalman:**
    * Aplicação de um Filtro de Kalman para fusão dos dados do acelerômetro e giroscópio.
    * Estimativa precisa dos ângulos de Roll (inclinação lateral) e Pitch (arfagem/inclinação frontal-traseira).
    * Redução de ruído e correção de desvios (drift) do giroscópio.

3.  **Armazenamento de Dados em Cartão SD:**
    * Interface com o módulo de cartão SD utilizando comunicação SPI.
    * Integração com a biblioteca FATFS para gerenciamento do sistema de arquivos.
    * Criação e escrita de arquivos `.csv` no cartão SD (`MPU_data1.csv` é um exemplo) para armazenar os dados do sensor e os ângulos filtrados.
    * Funções para inicializar o cartão, abrir/fechar arquivos e registrar as leituras de forma organizada.

4.  **Exibição em Display OLED SSD1306:**
    * Comunicação com o display OLED SSD1306 via I2C.
    * Exibição em tempo real dos valores de aceleração, giroscópio e dos ângulos de Roll e Pitch calculados.
    * Utilização de uma biblioteca customizada para o display (ssd1306.c/h e font.h) para renderização de texto e gráficos.

5.  **Calibração do MPU6050:**
    * Rotina de calibração para determinar e aplicar offsets aos dados do giroscópio e do acelerômetro, garantindo leituras mais precisas.

6.  **Análise de Dados Offline:**
    * Inclusão de um script Python (`plot_dados.py`) para ler os arquivos `.csv` gerados no cartão SD.
    * Visualização gráfica dos dados de aceleração, giroscópio e ângulos ao longo do tempo, facilitando a análise e depuração.

## 🛠️ Requisitos do Projeto
-   **Hardware:**
    -   Raspberry Pi Pico (ou outro microcontrolador RP2040)
    -   Sensor MPU6050 (Giroscópio e Acelerômetro 6-Axis)
    -   Módulo de Cartão SD (com interface SPI)
    -   Cartão Micro SD (formatado em FAT32)
    -   Display OLED SSD1306 (com interface I2C)
    -   Fios jumper, protoboard
-   **Software/Bibliotecas:**
    -   Raspberry Pi Pico SDK
    -   Bibliotecas para comunicação I2C e SPI (`hardware/i2c.h`, `hardware/spi.h`)
    -   Biblioteca FATFS (inclusa no projeto via `lib/FatFs_SPI`)
    -   Drivers customizados para MPU6050, SSD1306 e Filtro de Kalman (inclusos)
    -   Python 3 (para o script de plotagem)
    -   Bibliotecas Python: `pandas`, `matplotlib` (para `plot_dados.py`)
    -   Ferramentas de compilação (CMake, Make)
    -   IDE (Visual Studio Code com extensão Pico-Go, ou similar)

## 📂 Estrutura do Repositório
├── mpu_SD/
│   ├── Arquivos Salvos/    # Diretório para arquivos CSV de dados e scripts de plotagem
│   │   ├── MPU_data1.csv
│   │   └── plot_dados.py
│   ├── CMakeLists.txt      # Arquivo de build CMake para o projeto principal
│   ├── Cartao_FatFS_SPI.c  # Código principal que orquestra as leituras, filtragem e escrita no SD
│   ├── hw_config.c         # Configurações de hardware (pinos SPI, I2C, etc.)
│   ├── lib/                # Diretório para bibliotecas customizadas
│   │   ├── FatFs_SPI/      # Biblioteca FatFs para SD Card e drivers SPI
│   │   │   ├── ff15/       # Código fonte da FatFs
│   │   │   ├── include/    # Cabeçalhos da FatFs
│   │   │   └── sd_driver/  # Drivers de baixo nível para SD Card e SPI
│   │   ├── font.h          # Definições de fonte para o display OLED
│   │   ├── ssd1306.c       # Implementação do driver do display SSD1306
│   │   └── ssd1306.h       # Cabeçalho do driver do display SSD1306
│   ├── build/              # Diretório de build (gerado pelo CMake)
│   │   └── ...

## 🖥️ Como Compilar
1.  **Clone o repositório:**
    ```bash
    git clone https://github.com/JPSRaccolto/mpu_sd.git
    ```
2.  **Navegue até o diretório do projeto:**
    ```bash
    cd mpu_sd
    ```
3.  **Compile o projeto com seu ambiente de desenvolvimento configurado para o RP2040.**
    * Se estiver usando CMake/Make:
        ```bash
        mkdir build
        cd build
        cmake ..
        make
        ```
    * Isso gerará o arquivo `.uf2` na pasta `build`.

4.  **Carregue o código na placa Raspberry Pi Pico:**
    * Conecte sua Raspberry Pi Pico ao computador enquanto segura o botão `BOOTSEL`.
    * Um novo volume de armazenamento aparecerá no seu computador.
    * Arraste o arquivo `.uf2` gerado (por exemplo, `Usar_SSD.uf2` na pasta `build`) para este volume.
    * A Pico será reiniciada e começará a executar o firmware.

## 🖥️ Método Alternativo (VS Code):
1.  Baixe o repositório como um arquivo ZIP e extraia-o para uma pasta de fácil acesso.
2.  Abra o VS Code e certifique-se de ter a extensão "Pico-Go" ou "Raspberry Pi Pico" instalada.
3.  Abra a pasta do projeto (`mpu_SD`) no VS Code.
4.  Utilize o ícone "Build" (geralmente um martelo ou engrenagem) da extensão para compilar o projeto.
5.  Com a Raspberry Pi Pico em modo bootloader (segurando BOOTSEL ao conectar), utilize o ícone "Upload" (geralmente uma seta para cima) da extensão para enviar o programa para a sua RP2040.
6.  Após o upload, o sistema começará a operar, exibindo dados no OLED e gravando no SD.

## 🧑‍💻 Autor
**João Pedro Soares Raccolto**

## 📝 Descrição
Este projeto oferece uma solução completa para a aquisição e análise de dados de movimento utilizando o sensor MPU6050 e o microcontrolador Raspberry Pi Pico. A capacidade de registrar esses dados em um cartão SD, gerenciado pela biblioteca FATFS, é fundamental para o registro de longo prazo e a análise offline. A integração com um display OLED permite a visualização imediata de feedbacks visuais ao usuário. Por fim, o script Python de plotagem facilita a interpretação dos dados coletados, transformando números brutos em gráficos compreensíveis para análise de tendências e comportamentos de movimento.

## 🤝 Contribuições
Este projeto foi desenvolvido por **João Pedro Soares Raccolto**.
Contribuições são bem-vindas! Se você deseja contribuir, por favor, siga os passos abaixo:

1.  Faça um Fork deste repositório.
2.  Crie uma nova branch para sua feature:
    ```bash
    git checkout -b minha-nova-feature
    ```
3.  Faça suas modificações e commit:
    ```bash
    git commit -m 'Adiciona minha nova feature'
    ```
4.  Envie suas alterações para o seu Fork:
    ```bash
    git push origin minha-nova-feature
    ```
5.  Abra um Pull Request.

## 📽️ Demonstração em Vídeo
-   O vídeo de demonstração do projeto pode ser visualizado aqui: [Drive](https://drive.google.com/file/d/1LcwTsg2AsiAJPlvQJ6R4kndHqS9_Jhja/view?usp=sharing)

## 💡 Considerações Finais
Este projeto serve como uma base sólida para qualquer aplicação que exija o monitoramento e registro de dados inerciais. A combinação do MPU6050 com o armazenamento em SD e o processamento via Filtro de Kalman o torna uma ferramenta versátil para engenheiros, estudantes e entusiastas. Futuras melhorias podem incluir a adição de um módulo RTC (Real-Time Clock) para timestamping preciso dos dados, comunicação sem fio para monitoramento remoto, ou a integração com outros sensores para uma análise de contexto mais rica.
