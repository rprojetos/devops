verificar o usuário corrente, você pode utilizar o comando:
```bash
whoami
```

Para garantir que o usuário `user` tenha permissão total sobre o diretório `/home/user`, alterare a propriedade recursivamente com o seguinte comando:
Obs: Substitua <user> pelo nome do usuário
```bash
chown -R user:user /home/user

