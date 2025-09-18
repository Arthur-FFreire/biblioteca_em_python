# biblioteca_em_python
import json
import os
# Nome do arquivo JSON onde os livros serão armazenados
ARQUIVO = "biblioteca.json"
# Carregar ou criar
def carregar_dados():
    if os.path.exists(ARQUIVO) and os.path.getsize(ARQUIVO) > 0:
        with open(ARQUIVO, "r", encoding="utf-8") as f:
            try:
                return json.load(f)
            except Exception:
                print("Erro ao ler o arquivo JSON. Criando um banco novo.")
                return []
    return []
# Salvar dados
def salvar_dados(livros):
    with open(ARQUIVO, "w", encoding="utf-8") as f:
        json.dump(livros, f, indent=4, ensure_ascii=False)
# Cadastrar
def cadastrar_livro(livros):
    titulo = input("Título do livro: ")
    autor = input("Autor: ")
    while True:
        ano = input("Ano de publicação: ")
        if ano.isdigit():
            break
        else:
            print("Digite apenas números para o ano!")
    livro = {"titulo": titulo, "autor": autor, "ano": ano}
    livros.append(livro)
    salvar_dados(livros)
    print("Livro cadastrado com sucesso!")
# Listar
def listar_livros(livros):
    if not livros:
        print("Nenhum livro cadastrado.")
        return
    print("\nLista de Livros:")
    for i, livro in enumerate(livros, 1):
        print(f"{i} - {livro['titulo']} ({livro['autor']}, {livro['ano']})")
# Atualizar
def atualizar_livro(livros):
    listar_livros(livros)
    try:
        idx = int(input("Digite o número do livro que deseja atualizar: ")) - 1
        if 0 <= idx < len(livros):
            livro = livros[idx]
            livro["titulo"] = input("Novo título (Enter para manter): ") or livro["titulo"]
            livro["autor"] = input("Novo autor (Enter para manter): ") or livro["autor"]
            while True:
                novo_ano = input("Novo ano (Enter para manter): ")
                if not novo_ano:
                    break
                if novo_ano.isdigit():
                    livro["ano"] = novo_ano
                    break
                else:
                    print("Digite apenas números para o ano!")
            salvar_dados(livros)
            print("Livro atualizado com sucesso!")
        else:
            print("Livro não encontrado.")
    except ValueError:
        print("Número inválido.")
# Excluir
def excluir_livro(livros):
    listar_livros(livros)
    try:
        idx = int(input("Digite o número do livro que deseja excluir: ")) - 1
        if 0 <= idx < len(livros):
            livros.pop(idx)
            salvar_dados(livros)
            print("Livro excluído com sucesso!")
        else:
            print("Livro não encontrado.")
    except ValueError:
        print("Número inválido.")
# Menu
def menu():
    livros = carregar_dados()
    while True:
        print("\n===== Biblioteca =====")
        print("1. Cadastrar livro")
        print("2. Listar livros")
        print("3. Atualizar livro")
        print("4. Excluir livro")
        print("5. Sair")
        opcao = input("Escolha uma opção: ")
        if opcao == "1":
            cadastrar_livro(livros)
        elif opcao == "2":
            listar_livros(livros)
        elif opcao == "3":
            atualizar_livro(livros)
        elif opcao == "4":
            excluir_livro(livros)
        elif opcao == "5":
            print("Saindo do sistema...")
            break
        else:
            print("Opção inválida.")
# Executar
if __name__ == "__main__":
    menu()
