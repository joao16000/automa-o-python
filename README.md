# automa-o-python
Projeto automação Python
principais caracteristicas
automção do processo de cadastro de milhares de produtos no sistemas usando Python, para execução diária e sob demanda.
import pyautogui
import pandas as pd
import time

# ============================================================
# CONFIGURAÇÃO
# ============================================================
# define o tempo de espera entre os comandos do pyautogui
pyautogui.PAUSE = 0.5

# link do sistema onde vamos fazer o login e cadastro
URL_SISTEMA = "https://dlp.hashtagtreinamentos.com/python/intensivao/login"

# IMPORTANTE: as posições (x, y) abaixo são só um EXEMPLO.
# Você precisa rodar o "pegar_posicao.py" e substituir os valores
# pelas coordenadas reais da SUA tela, senão os cliques vão cair
# no lugar errado.

POS_CAMPO_EMAIL = (1686, 591)
POS_BOTAO_LOGAR = (1902, 838)

POS_PRIMEIRO_CAMPO_FORMULARIO = (1723, 418)  # campo "codigo" do produto
POS_BOTAO_ENVIAR = (1808, 1380)

# ============================================================
# 1) IMPORTAR A BASE DE DADOS
# ============================================================
tabela = pd.read_csv("produtos.csv")
print(tabela)

# ============================================================
# 2) ABRIR O NAVEGADOR E ACESSAR O SISTEMA
# ============================================================
pyautogui.press("win")
pyautogui.write("chrome")
pyautogui.press("enter")
time.sleep(2)  # espera o chrome abrir

pyautogui.write(URL_SISTEMA)
pyautogui.press("enter")

time.sleep(5)  # espera o site carregar

# ============================================================
# 3) FAZER LOGIN NO SISTEMA
# ============================================================
pyautogui.click(x=POS_CAMPO_EMAIL[0], y=POS_CAMPO_EMAIL[1])
pyautogui.write("pythonimpressionador@gmail.com")
pyautogui.press("tab")
pyautogui.write("Tiger900@")
pyautogui.click(x=POS_BOTAO_LOGAR[0], y=POS_BOTAO_LOGAR[1])

time.sleep(3)  # espera a página do formulário carregar

# ============================================================
# 4) CADASTRAR TODOS OS PRODUTOS DA TABELA
# ============================================================
for linha in tabela.index:
    pyautogui.click(x=POS_PRIMEIRO_CAMPO_FORMULARIO[0], y=POS_PRIMEIRO_CAMPO_FORMULARIO[1])  # clica no 1º campo

    pyautogui.write(str(tabela.loc[linha, "codigo"]))          # código do produto
    pyautogui.press("tab")

    pyautogui.write(str(tabela.loc[linha, "marca"]))           # marca
    pyautogui.press("tab")

    pyautogui.write(str(tabela.loc[linha, "tipo"]))            # tipo
    pyautogui.press("tab")

    pyautogui.write(str(tabela.loc[linha, "categoria"]))       # categoria
    pyautogui.press("tab")

    pyautogui.write(str(tabela.loc[linha, "preco_unitario"]))  # preço unitário
    pyautogui.press("tab")

    pyautogui.write(str(tabela.loc[linha, "custo"]))           # custo
    pyautogui.press("tab")

    # só preenche a observação se ela existir (não for vazia/NaN)
    if not pd.isna(tabela.loc[linha, "obs"]):
        pyautogui.write(str(tabela.loc[linha, "obs"]))

    # clica no botão de enviar para registrar o produto
    pyautogui.click(x=POS_BOTAO_ENVIAR[0], y=POS_BOTAO_ENVIAR[1])

    # se o formulário precisar rolar entre um cadastro e outro,
    # descomente a linha abaixo e ajuste o valor do scroll
    # pyautogui.scroll(5000)

print("Cadastro finalizado!")
