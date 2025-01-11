# Sistema de Monitoramento de Tentativas de Invasão

Este projeto implementa um sistema de monitoramento de segurança que analisa registros de log de tentativas de acesso a um sistema e detecta possíveis invasões. O sistema emite um alerta quando detecta mais de 3 tentativas consecutivas de falha para o mesmo usuário.

## Funcionalidade

A função `detectar_invasao` percorre os registros de log e verifica se algum usuário teve mais de 3 tentativas consecutivas de falha. Se um invasor for detectado, o sistema retorna o ID do usuário invasor. Caso contrário, retorna a mensagem "Nenhum invasor detectado".

## Como Funciona

### Passos:
1. O código recebe uma entrada de logs no formato `id_usuario:status`, onde:
   - `id_usuario` é o identificador do usuário.
   - `status` pode ser `falha` ou `sucesso`.

2. O código percorre cada registro e conta as falhas consecutivas para cada usuário.

3. Se um usuário tiver mais de 3 falhas consecutivas, ele é identificado como possível invasor e o ID do usuário é retornado.

4. Se nenhum invasor for detectado, a mensagem "Nenhum invasor detectado" é retornada.

## Código

```python
import sys

def detectar_invasao(registros):
    # Variáveis para rastrear o ID do usuário atual e suas falhas consecutivas
    usuario_atual = None
    tentativas_consecutivas = 0

    # Percorre cada registro de log
    for registro in registros:
        # Separa o ID do usuário e o status do registro (sucesso ou falha)
        id_usuario, status = registro.split(":")
        id_usuario = id_usuario.strip()
        status = status.strip()

        # Verifica se o usuário atual é o mesmo da iteração anterior
        if id_usuario == usuario_atual:
            if status == "falha":
                tentativas_consecutivas += 1
                # Se o usuário teve mais de 3 falhas consecutivas, retorne o invasor
                if tentativas_consecutivas > 3:
                    return id_usuario
            else:
                # Se a tentativa foi bem-sucedida, reseta o contador de falhas
                tentativas_consecutivas = 0
        else:
            # Atualiza para o novo usuário e reinicia a contagem de tentativas falhas
            usuario_atual = id_usuario
            tentativas_consecutivas = 1 if status == "falha" else 0

    # Se nenhum invasor for detectado, retorna a mensagem padrão
    return "Nenhum invasor detectado"

# Função principal para executar o programa
def main():
    # Lê os registros de log da entrada padrão
    entrada = sys.stdin.readline().strip()
    
    if not entrada:
        print("Nenhum invasor detectado")
        return

    # Processa os registros para garantir que sejam válidos
    registros = [registro.strip() for registro in entrada.split(",") if registro]

    # Chama a função para detectar tentativas de invasão
    resultado = detectar_invasao(registros)

    # Imprime o resultado na saída padrão
    print(resultado)

# Chama a função principal para executar o programa
if __name__ == "__main__":
    main()
```

## Como Usar

1. Clone o repositório:
   ```bash
   git clone https://github.com/seu_usuario/seu_repositorio.git
   cd seu_repositorio
   ```

2. Execute o código passando os registros de log através da entrada padrão:
   ```bash
   python seu_arquivo.py
   ```

3. Exemplo de entrada:
   ```
   user1:falha, user2:falha, user2:falha, user2:falha
   ```

   Saída esperada:
   ```
   user2
   ```

## Testes

O código foi testado com diferentes conjuntos de dados de entrada, incluindo casos onde:
- Não há invasores.
- Um usuário fez mais de 3 tentativas consecutivas de falha.

### Exemplo 1
**Entrada**:
```
user1:falha, user1:falha, user1:falha, user1:sucesso
```

**Saída esperada**:
```
Nenhum invasor detectado
```

### Exemplo 2
**Entrada**:
```
user2:falha, user2:falha, user2:falha, user2:falha
```

**Saída esperada**:
```
user2
```

### Exemplo 3
**Entrada**:
```
user3:sucesso, user3:falha, user3:falha, user3:falha, user3:falha
```

**Saída esperada**:
```
user3
```

## Contribuições

Sinta-se à vontade para abrir issues ou enviar pull requests se você quiser melhorar o projeto!

## Licença

Este projeto está licenciado sob a Licença MIT - veja o arquivo [LICENSE](https://opensource.org/license/mit) para mais detalhes.
