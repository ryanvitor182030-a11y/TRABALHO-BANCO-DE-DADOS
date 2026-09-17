# Sistema de Gestão de Marcenaria
# Projeto acadêmico - sem banco de dados

clientes = []
produtos = []
pedidos = []


# -------------------------
# CADASTRO DE CLIENTES
# -------------------------

def cadastrar_cliente():
    id_cliente = len(clientes) + 1
    nome = input("Nome do cliente: ")
    telefone = input("Telefone: ")
    email = input("E-mail: ")

    cliente = {
        "id": id_cliente,
        "nome": nome,
        "telefone": telefone,
        "email": email
    }

    clientes.append(cliente)
    print("Cliente cadastrado com sucesso!")


def listar_clientes():
    if not clientes:
        print("Nenhum cliente cadastrado.")
        return

    for cliente in clientes:
        print(
            f"ID: {cliente['id']} | "
            f"Nome: {cliente['nome']} | "
            f"Telefone: {cliente['telefone']} | "
            f"E-mail: {cliente['email']}"
        )


# -------------------------
# CADASTRO DE PRODUTOS
# -------------------------

def cadastrar_produto():
    id_produto = len(produtos) + 1
    nome = input("Nome do produto: ")
    descricao = input("Descrição: ")

    try:
        preco = float(input("Preço: R$ "))
        if preco < 0:
            print("O preço não pode ser negativo.")
            return
    except ValueError:
        print("Digite um preço válido.")
        return

    produto = {
        "id": id_produto,
        "nome": nome,
        "descricao": descricao,
        "preco": preco
    }

    produtos.append(produto)
    print("Produto cadastrado com sucesso!")


def listar_produtos():
    if not produtos:
        print("Nenhum produto cadastrado.")
        return

    for produto in produtos:
        print(
            f"ID: {produto['id']} | "
            f"Nome: {produto['nome']} | "
            f"Descrição: {produto['descricao']} | "
            f"Preço: R$ {produto['preco']:.2f}"
        )


# -------------------------
# CADASTRO DE PEDIDOS
# -------------------------

def cadastrar_pedido():
    if not clientes:
        print("Cadastre pelo menos um cliente antes de criar um pedido.")
        return

    if not produtos:
        print("Cadastre pelo menos um produto antes de criar um pedido.")
        return

    listar_clientes()

    try:
        id_cliente = int(input("Digite o ID do cliente: "))
    except ValueError:
        print("ID inválido.")
        return

    cliente = next(
        (c for c in clientes if c["id"] == id_cliente),
        None
    )

    if cliente is None:
        print("Cliente não encontrado.")
        return

    itens = []

    while True:
        listar_produtos()

        try:
            id_produto = int(input("Digite o ID do produto: "))
            quantidade = int(input("Digite a quantidade: "))

            if quantidade <= 0:
                print("A quantidade deve ser maior que zero.")
                continue
        except ValueError:
            print("Digite valores válidos.")
            continue

        produto = next(
            (p for p in produtos if p["id"] == id_produto),
            None
        )

        if produto is None:
            print("Produto não encontrado.")
            continue

        item = {
            "id_produto": produto["id"],
            "nome_produto": produto["nome"],
            "quantidade": quantidade,
            "valor_unitario": produto["preco"],
            "subtotal": quantidade * produto["preco"]
        }

        itens.append(item)

        continuar = input("Adicionar outro produto? (s/n): ").lower()
        if continuar != "s":
            break

    valor_total = sum(item["subtotal"] for item in itens)

    pedido = {
        "id": len(pedidos) + 1,
        "id_cliente": cliente["id"],
        "nome_cliente": cliente["nome"],
        "status": "Orçamento",
        "itens": itens,
        "valor_total": valor_total
    }

    pedidos.append(pedido)

    print(f"Pedido criado com sucesso! Total: R$ {valor_total:.2f}")


def listar_pedidos():
    if not pedidos:
        print("Nenhum pedido cadastrado.")
        return

    for pedido in pedidos:
        print("\n-------------------------")
        print(f"Pedido: {pedido['id']}")
        print(f"Cliente: {pedido['nome_cliente']}")
        print(f"Status: {pedido['status']}")
        print(f"Valor total: R$ {pedido['valor_total']:.2f}")

        print("Itens:")
        for item in pedido["itens"]:
            print(
                f"- {item['nome_produto']} | "
                f"Quantidade: {item['quantidade']} | "
                f"Subtotal: R$ {item['subtotal']:.2f}"
            )


def atualizar_status_pedido():
    if not pedidos:
        print("Nenhum pedido cadastrado.")
        return

    listar_pedidos()

    try:
        id_pedido = int(input("\nDigite o ID do pedido: "))
    except ValueError:
        print("ID inválido.")
        return

    pedido = next(
        (p for p in pedidos if p["id"] == id_pedido),
        None
    )

    if pedido is None:
        print("Pedido não encontrado.")
        return

    status_permitidos = [
        "Orçamento",
        "Aguardando aprovação",
        "Aprovado",
        "Em produção",
        "Pronto",
        "Entregue",
        "Cancelado"
    ]

    print("\nStatus disponíveis:")
    for indice, status in enumerate(status_permitidos, start=1):
        print(f"{indice} - {status}")

    try:
        opcao = int(input("Escolha o novo status: "))
        pedido["status"] = status_permitidos[opcao - 1]
        print("Status atualizado com sucesso!")
    except (ValueError, IndexError):
        print("Opção inválida.")


# -------------------------
# MENU PRINCIPAL
# -------------------------

def menu():
    while True:
        print("\n==============================")
        print(" SISTEMA DE GESTÃO - MARCENARIA")
        print("==============================")
        print("1 - Cadastrar cliente")
        print("2 - Listar clientes")
        print("3 - Cadastrar produto")
        print("4 - Listar produtos")
        print("5 - Criar pedido")
        print("6 - Listar pedidos")
        print("7 - Atualizar status do pedido")
        print("0 - Sair")

        opcao = input("Escolha uma opção: ")

        if opcao == "1":
            cadastrar_cliente()
        elif opcao == "2":
            listar_clientes()
        elif opcao == "3":
            cadastrar_produto()
        elif opcao == "4":
            listar_produtos()
        elif opcao == "5":
            cadastrar_pedido()
        elif opcao == "6":
            listar_pedidos()
        elif opcao == "7":
            atualizar_status_pedido()
        elif opcao == "0":
            print("Sistema encerrado.")
            break
        else:
            print("Opção inválida.")


if __name__ == "__main__":
    menu()
