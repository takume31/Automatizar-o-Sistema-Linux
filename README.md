<h1>AUTOMAÇÃO DE TAREFAS COM SHELL SCRIPT: COMO O LINUX FACILITA A ROTINA DE TI</h1>

<li>
  <h3>
  Introdução
  </h3>
</li>

Neste procedimento, será apresentado o processo de criação e configuração de um script Bash destinado à automação de tarefas e à melhoria da usabilidade do terminal Linux por meio de um menu interativo. Inicialmente, será criada uma pasta específica para organização dos scripts, seguida da edição do arquivo responsável pelo menu utilizando o editor nano.

O script desenvolvido tem como objetivo central facilitar a execução de operações administrativas e rotineiras, oferecendo uma interface textual intuitiva, com uso de cores e efeitos visuais. Entre suas funcionalidades, destacam-se a atualização do sistema, limpeza de cache, realização de backups, instalação automatizada de aplicativos populares, personalização do prompt do terminal (PS1), edição e recarregamento do arquivo .bashrc, além de opções para reiniciar ou desligar o computador.

Adicionalmente, o script inclui submenus especializados, como o menu de temas, que permite alterar dinamicamente a aparência do terminal, e o menu de instalação, que automatiza downloads e instalações utilizando ferramentas como apt, snap, wget e curl. Por fim, será demonstrado como configurar um alias no .bashrc, possibilitando a execução do **menu** simplesmente ao digitar o comando menu no terminal, tornando o uso mais rápido e eficiente.
<p>
  No Terminal
</p>
  
  <img src="/terminal-logo.png" width="200">

Iniciaremos criando uma pasta chamada 'scripts', utilizando o comando **mkdir**.
  
    mkdir scripts

Utilizaremos o comando **nano ~/scripts/menu.sh**. Esse comando abrirá a pasta ‘scripts’ e, caso o arquivo **menu.sh** ainda não exista, ele será criado automaticamente. Se o arquivo já existir, o comando abrirá o editor para edição do mesmo.
  
    nano ~/scripts/menu.sh

Esse script cria um menu interativo no terminal para facilitar tarefas comuns no Linux. Ele usa cores e efeitos visuais, permitindo atualizar o sistema, limpar cache, fazer backup, instalar aplicativos populares, alterar o tema do terminal (PS1), editar/recarregar o **.bashrc** e reiniciar ou desligar o computador.

Além disso, oferece um submenu de temas que aplica cores diferentes ao prompt do terminal e um submenu de instalação que automatiza downloads e instalações via apt, snap, wget e curl.
            
    #!/bin/bash
    WIDTH=$(tput cols)
    HEIGHT=$(tput lines)

    #Importações de Cores
    RED='\e[91m'
    PINK='\e[38;5;205m'
    GREEN='\e[92m'
    BLUE='\e[94m'
    MAGENTA='\e[95m'
    CYAN='\e[96m'
    YELLOW='\e[93m'
    RESET='\e[0m'
    BLINK='\e[5m'

    # importações deos temas
    # Tema Rosa
    estilo_rosa() {
        echo "" >> ~/.bashrc
          echo "# Tema Rosa" >> ~/.bashrc
          echo "PS1=\"${PINK}\u@\h:\w → ${RESET}\"" >> ~/.bashrc
      }
  
      # Tema Verde
        estilo_verde() {
            echo "" >> ~/.bashrc
            echo "# Tema Verde" >> ~/.bashrc
            echo "PS1=\"${GREEN}\u@\h:\w → ${RESET}\"" >> ~/.bashrc
      }

    # Tema Azul
    estilo_azul() {
        echo "" >> ~/.bashrc
        echo "# Tema Azul" >> ~/.bashrc
        echo "PS1=\"${BLUE}\u@\h:\w → ${RESET}\"" >> ~/.bashrc
    }
    # Tema Roxo
    estilo_roxo() {
        echo "" >> ~/.bashrc
        echo "# Tema Roxo" >> ~/.bashrc
        echo "PS1=\"${MAGENTA}\u@\h:\w → ${RESET}\"" >> ~/.bashrc
    }

    # Alterar Seu Tema
    menu_tematico() {
        while true; do
            clear
            echo -e "${MAGENTA}=== TEMAS DO TERMINAL ===${RESET}"
            echo -e "1) ${PINK}Rosa${RESET}"
            echo -e "2) ${GREEN}Verde${RESET}"
            echo -e "3) ${BLUE}Azul${RESET}"
            echo -e "4) ${MAGENTA}Roxo${RESET}"
            echo -e "ESC) Voltar"
            read -s -n1 opc
            echo ""
            case $opc in
                1)
                    estilo_rosa
                    echo -e "\n${PINK}${BLINK} Tema Rosa aplicado! ${RESET}"
                ;;
                2)
                    estilo_verde
                    echo -e "\n${GREEN}${BLINK} Tema Verde aplicado! ${RESET}"
                ;;
                3)
                    estilo_azul
                    echo -e "\n${BLUE}${BLINK} Tema Azul aplicado! ${RESET}"
                ;;
                4)
                    estilo_roxo
                    echo -e "\n${MAGENTA}${BLINK} Tema Roxo aplicado! ${RESET}"
                ;;
                $'\e')
                    break
                ;;
                *)
                    echo "Opção inválida!"
                ;;
            esac
            source ~/.bashrc
            read -p "Pressione ENTER para continuar..."
        done
    }

    # Dowloads
    instalar_apps() {
        while true; do
            clear
            echo -e "${CYAN}=== INSTALAÇÃO DE APLICATIVOS ===${RESET}"
            echo "1) Google Chrome"
            echo "2) VS Code"
            echo "3) Node.js (LTS)"
            echo "4) VLC"
            echo "5) GIMP"
            echo "6) PostgreSQL"
            echo "7) pgAdmin 4"
            echo "ESC) Voltar"
            read -s -n1 opc
            case $opc in
                1)
                    wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
                    sudo dpkg -i google-chrome-stable_current_amd64.deb
                    sudo apt install -f -y
                    rm google-chrome-stable_current_amd64.deb
                    ;;
                2) 
                   sudo snap install code --classic ;;
                3)
                   curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -
                    sudo apt install -y nodejs
                    ;;
                4)
                   sudo apt install -y vlc ;;
                5) 
                   sudo apt install -y gimp ;;
                6) 
                   sudo apt install -y postgresql postgresql-contrib ;;
                7)
                    curl https://www.pgadmin.org/static/packages_pgadmin_org.pub | sudo gpg --dearmor -o /usr/share/keyrings/pgadmin.gpg
                    sudo sh -c 'echo "deb [signed-by=/usr/share/keyrings/pgadmin.gpg] https://ftp.postgresql.org/pub/pgadmin/pgadmin4/apt/focal pgadmin4 main" > /etc/apt/sources.list.d/pg>
                    sudo apt update
                    sudo apt install -y pgadmin4
                    ;;
                $'\e') break ;;
                *) echo "Opção inválida!";;
            esac
            read -p "Pressione ENTER para continuar..."
        done
    }

    # Menu
    while true; do
        clear
        echo -e "${MAGENTA}${BLINK}══════════════════════════════════════${RESET}"
        echo -e "${CYAN}         MENU ULTRA GAMER ${RESET}"
        echo -e "${MAGENTA}${BLINK}══════════════════════════════════════${RESET}"
        echo ""
        echo -e "${CYAN}1)${RESET} Atualizar sistema"
        echo -e "${CYAN}2)${RESET} Limpar cache"
        echo -e "${CYAN}3)${RESET} Backup"
        echo -e "${CYAN}4)${RESET} Instalar aplicativos"
        echo -e "${CYAN}5)${RESET} Estilizar Terminal"
        echo -e "${CYAN}6)${RESET} Editar .bashrc"
        echo -e "${CYAN}7)${RESET} Reload .bashrc"
        echo -e "${CYAN}8)${RESET} Reiniciar"
        echo -e "${CYAN}9)${RESET} Desligar"
        echo -e "${CYAN}ESC)${RESET} Sair"
        echo ""
        read -s -n1 opc
        case         $opc in
            1) 
                sudo apt update && sudo apt upgrade -y 
            ;;
            2) 
                sudo apt autoremove -y && sudo apt clean 
            ;;
            3) 
                tar -czvf backup_$(date +"%d-%m-%Y").tar.gz ~ 
            ;;
            4) 
                instalar_apps 
            ;;
            5) 
                menu_tematico 
            ;;
            6)
                nano ~/.bashrc 
            ;;
            7) 
                source ~/.bashrc 
            ;;
            8) 
                reboot 
            ;;
            9) 
                poweroff 
            ;;
            $'\e') 
                exit 
            ;;
        esac
        read -p "Pressione ENTER para continuar..."
    done

### Vamos configurar para nos usarmos o menu se nos escrevermos no Terminal:

    menu

Abrir o arquivo  ~/.bashrc

    nano ~/.bashrc

na última linha insira esse código:

    alias menu="bash ~/scripts/menu.sh"
    
Após inserir o comando na última linha, pressione:

Para salvar o arquivo:
Pressione Ctrl + O

Para sair do arquivo e retornar ao terminal:
Pressione Ctrl + X
    
Para configurar o comando de forma que o menu seja executado ao digitar apenas menu no terminal, adicione o seguinte comando:
    
    source ~/.bashrc
    
