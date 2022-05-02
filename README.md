# bd1
Atividade Banco de Dados I

# -*- coding: utf-8 -*-
#-*- Guilherme Vecentin e Guilherme Dalabrida -*-

import sqlite3 
import sys
import time

def conectarBanco():
    conexao = None
    try:
        conexao = sqlite3.connect(r".\trabsson.db",timeout=10)
        return conexao
    except sqlite3.Error as e:
        print(e)
        return e
    return conexao
    
def criarBanco(conexao):
    con = conexao.execute("CREATE TABLE automovel (codigo INTEGER PRIMARY KEY AUTOINCREMENT,modelo_do_carro TEXT,ano TEXT,tabela_fipe REAL (10,2));")
    conexao.commit()
    if(con):
        print("\nTabela Criada")
        

def insertBanco(conexao,carro,ano,fipe):
    try:
        con = conexao.execute(f'INSERT INTO automovel (modelo_do_carro,ano,tabela_fipe) VALUES ("{carro}","{ano}",{fipe});')
        conexao.commit()
        if(con):
            print("\nInserção Concluída")
    except sqlite3.Error as error:
        print("\nFailed to insert data in sqlite table", error)
        
        
def deleteBanco(conexao,codigo):
    try:
        cursor = conexao.cursor()
        con = cursor.execute(f'DELETE FROM automovel WHERE codigo = {codigo};')
        conexao.commit()
        if(con):
            print("Exclusão Concluída")
    except sqlite3.Error as error:
        print("Failed to delete data from sqlite table", error)
        
    
def printBanco(conexao):
    cursor = conexao.cursor()
    cursor.execute("SELECT * FROM automovel;")
    registros = cursor.fetchall()
    for registro in registros:
        print("------------------------")
        print("Opção : " , registro[0])
        print("Modelo do Carro : " , registro[1])
        print("Ano : " , registro[2])
        print("Tabela Fipe : R$" , registro[3])
        print("------------------------")
    cursor.close()
    

def dropTabela(conexao, tabela):
    conexao.execute("DROP TABLE " + tabela + ";")
    conexao.commit()
    
    
def insertTabela():
    carro = input("Modelo do Carro: ")
    ano = input("Ano: ")
    fipe = input("Tabela Fipe: ")
    confi = input("Modelo do Carro : " + carro +"\n"
                  +"Ano : " + ano + "\n"
                  +"Tabela Fipe : " + fipe + "\n"
                  +"\nDeseja Continuar? \n"
                  +"Resposta (S/N) : ")
    if(confi.upper() == "S"):
        insertBanco(conexao,carro,ano,fipe)


def excluirTabela():
    printBanco(conexao)
    id = int(input("Qual opção deseja excluir? \n"
                   +" Opção : "))
    cursor = conexao.cursor()
    cursor.execute(f'SELECT * FROM automovel WHERE codigo = {id};')
    registros = cursor.fetchall()
    for registro in registros:
        print("------------------------")
        print("Opção : " , registro[0])
        print("Modelo do Carro : " , registro[1])
        print("Ano : " , registro[2])
        print("Tabela Fipe : R$" , registro[3])
        print("------------------------")
    cursor.close()
    confi = input("Gostaria de Continuar? \n"
                 +"Resposta (S/N) : ")
    
    if(confi.upper() == "S"):
        deleteBanco(conexao, id)
    
def updateTabela():
    printar()
    id = input("Qual opção deseja editar? : ")
    campo = input("Escolha o campo que deseja alterar : \n"
                  +" 1 - Modelo do Carro \n"
                  +" 2 - Ano \n"
                  +" 3 - Tabela Fipe \n"
                  +" 4 - Sair"
                  +" Opção : ")
    
    valor = input("Digite o novo valor : ")
    if(campo == "1"):
        campo = "modelo_do_carro"
    elif(campo == "2"):
        campo = "ano"
    elif(campo == "3"):
        campo = "tabela_fipe"
    elif(campo == "4"):
        sys.exit()
    else:
        pass
    updateBanco(conexao, campo, valor, id)
    
def updateBanco(conexao,campo,valor,id):
    conexao.execute(f'UPDATE automovel SET "{campo}"="{valor}" WHERE codigo={id};')

def printar():
    printBanco(conexao)
    
def menu():
    while(1<2):
        guia = input("Escolha uma opção : \n"
                      +" 1 - Inserir Valores \n"
                      +" 2 - Excluir Valores \n"
                      +" 3 - Printar Valores \n"
                      +" 4 - Alterar Valores \n"
                      +" 5 - Sair \n"
                      +" Opção: ")
        
        guia = int(guia)
        if(guia >= 1 and guia <= 5):
            if (guia == 1):
                insertTabela()
            elif(guia == 2):
                excluirTabela()
            elif(guia == 3):
                printBanco(conexao)
                time.sleep(3)
            elif(guia == 4):
                updateTabela()
            elif(guia==5):
                sys.exit()
            
        else:
            print()
            print("Valor Inválido")
            time.sleep(3)

conexao = conectarBanco()   
menu()
