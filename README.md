<h1>AUTOMAÇÃO DE TAREFAS COM SHELL SCRIPT: COMO O LINUX FACILITA A ROTINA DE TI</h1>

<li>
  <h3>
  Introdução
  </h3>
</li>
<p>
  A automação de tarefas é um dos principais fatores que tornam os sistemas Linux amplamente utilizados na área de Tecnologia da Informação. Por meio de Shell Script, é possível reunir comandos, rotinas de manutenção e configurações em um único arquivo executável, reduzindo erros manuais e aumentando a produtividade.
</p>
  <p>
  Este relatório apresenta a análise de um script em Bash, desenvolvido para o sistema Ubuntu Linux, que automatiza tarefas comuns da rotina de TI por meio de um menu interativo no terminal.
</p>
<p>
  No Terminal
</p>
<img src="https://brandslogos.com/wp-content/uploads/images/large/terminal-logo.png" 
  style={ 
  width="150", 
  height="100",  
  }/>
  
Primeiro iremos criar uma pasta chamada **scripts**:
  
    mkdir scripts

Após criar a pasta vamos usar o comando **nano** para criar ou entrar num arquivo ja existente e vamos chamar de **menu.sh**:
  
    nano ~/scripts/menu.sh

Quando você entar no arquivo coloca o Script
            
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

Entramos no arquivo bashrc:

    nano ~/.bashrc

na última linha insira esse código:

    alias menu="bash ~/scripts/menu.sh"
    
Após Inserir o comando na última linha vamos aperta 

Para salvar: 

    CTRL + O
    
Para opara sair do arquivo e voltar ao Terminal:

    CTRL + X
    
Para que o Script possa atualizar:
    
    source ~/.bashrc
    
