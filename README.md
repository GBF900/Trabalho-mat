# Trabalho-mat
```
import matplotlib.pyplot as plt

def menu():
    print("\nMenu:")
    print("1 - CADASTRO")
    print("2 - FINANÇAS (SALDO MENSAL E MACROANÁLISE DE DADOS)")
    print("3 - PROJEÇÕES (MICROANÁLISE DE GANHOS E DESPESAS)")
    print("4 - REAJUSTE DE FINANÇAS (EXCLUSÃO, AUMENTO E REDUÇÃO DE DESPESAS E GANHOS)")
    print("5 - LISTAGEM")
    print("0 - SAIR")
    op = input("INFORME SUA DECISÃO: ")
    while not op.isdigit() or int(op) not in range(0, 6):
        print("Opção inválida! Tente novamente.")
        op = input("Escolha uma opção: ")
    return int(op)


def cadastro( vet_cad, quant):
    while True:
        nome = input("NOME COMPLETO:  ").strip()
        if nome.replace(" ", "").isalpha(): 
            break
        print("NOME INVÁLIDO.TENTE NOVAMENTE.")

    while True:
        uf = input("UNIDADE FEDERAL (UF):  ").strip().upper()
        if len(uf) == 2 and uf.isalpha():
            break
        print("UNIDADE INVÁLIDA. TENTE NOVAMENTE. Digite o estado como, por exemplo, 'ES'.")

    while True:
        email = input("E-MAIL: ").strip()
        if "@" in email and "." in email.split("@")[-1]:  
            break
        print("E-MAIL INVÁLIDO. TENTE NOVAMENTE.")

    senha = input("SENHA: ").strip()

    usuario = input("USUÁRIO: ").strip()

    vet_cad.append({
        "NOME": nome,
        "UF": uf,
        "E-MAIL": email,
        "SENHA": senha,
        "USUARIO": usuario
    })
    print("Seu cadastro foi bem-sucedido!")
    quant+=1
    return vet_cad


import matplotlib.pyplot as plt
def finanças(vet_div, n_div):
    val = 0
    receb = 0
    sal = 0
    resto = []
    meses = ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec']
    
    while val <= 0:
        val = float(input("INFORME O TOTAL DAS SUAS DEPESAS PADRÕES (EM R$): "))
        if val <= 0:
            print("Valor inválido. Você precisa informar uma fonte de despesas acima de 0.")
    
    while receb <= 0:  
        receb = float(input("INFORME SEUS GANHOS TOTAIS PADRÕES (EM R$): "))
        if receb <= 0:
            print("Valor inválido. Você precisa informar uma fonte de ganhos acima de 0.")
            
    sal = receb - val  
    
    print("INDIQUE O MES PELO NÚMERO CORRESPONDENTE.\t Ex: 1- JANEIRO \t 2- FEVEREIRO \t 3- MARÇO\t")
    mes = int(input("INDIQUE-O SIMBOLIZANDO QUANDO VC GOSTARIA DE COMEÇAR A PROGRAMAR SUAS FINANÇAS: "))
   
    while mes < 1 or mes > 12:
        print("Mês inválido. Por favor, informe um número entre 1 e 12.")
        mes = int(input("INDIQUE O MÊS O QUAL VC GOSTARIA DE COMEÇAR A PROGRAMAR SUAS FINANÇAS:  \n"))
   
    for i in range(12 - (mes - 1)):
        resto.append((i + 1) * sal) 

    if sal > 0:
        print("-"*40)
        print("Seu saldo mensal corresponde a um valor positivo de R$ %.2f " % sal)
        print("Esse valor é o equivalente a sua saúde financeira em nível adequado.")
        print("EXPECTATIVA DE ECONOMIAS ATÉ O FINAL DO ANO: R$ %.2f \n" % resto[-1])
        graf(meses, mes, resto, sal)
       
    else:
        print("Você produz uma dívida mensal de R$ %.2f \n" % sal)
        print("Você precisa reduzir gastos. \n")
        graf(meses, mes, resto, sal)
        print("EXPECTATIVA DE DÍVIDAS ACUMULADAS ATÉ O FINAL DO ANO: R$ %.2f \n" % sum(resto))

    vet_div.append({'SALDO': sal, 'SALDO CUMULADO': resto})
    n_div += 1


def graf(meses, mes, resto, sal):
    meses_restantes =[meses[i] for i in range(mes - 1, len(meses))]
    plt.bar(meses_restantes, resto, color='b' if sal > 0 else 'r')
    plt.axhline(y=resto[0], color='g', linestyle='--', label='Saldo Inicial: R$ %.2f' % resto[0])
    plt.axhline(y=resto[-1], color='orange', linestyle='-', label='Saldo Final: R$ %.2f' % resto[-1])
    plt.xlabel('Meses')
    plt.ylabel('Saldo Acumulado (em R$)')
    plt.title('Progressão das Economias' if sal > 0 else 'Progressão das Dívidas')
    plt.legend()
    plt.grid(axis='y')
    plt.tight_layout()
    plt.show()


import matplotlib.pyplot as plt

def valores(val, receb, vet_gastos, vet_renda, n_d, n_r):
    while val <=0 or val=="":
        val = float(input("INFORME O TOTAL DAS SUAS DEPESAS PADRÕES (EM R$): "))
        if val <= 0:
            print("Valor inválido. Você precisa informar uma fonte de despesas acima de 0.")
    
    while receb <= 0 or receb=="":  
        receb = float(input("INFORME SEUS GANHOS TOTAIS PADRÕES (EM R$): "))
        if receb <= 0:
            print("Valor inválido. Você precisa informar uma fonte de ganhos acima de 0.")
            
    while n_d <= 0 or n_d=="":
            n_d = int(input("QUAL O NÚMERO DE GASTOS: "))
            if n_d <= 0:
                print("Por favor, insira um valor positivo para o número de gastos.")
    
    print("INFORME OS GASTOS NOMINAIS ABAIXO: \n")
    for i in range(n_d):
        while True:
                dep = float(input("GASTO %d: "%(i+1)))
                vet_gastos.append(dep)
                break
            
    while sum(vet_gastos) != val :
        print("VALORES INSERIDOS NÃO COINCIDEM COM O VALOR REFERENTE ÀS DESPESAS BRUTAS.")
        print("TOTAL INFORMADO: R$ %.2f\n"%val)
        vet_gastos.clear()
        for i in range(n_d):
            while True:
                    dep = float(input("GASTO %d: "%(i+1)))
                    vet_gastos.append(dep)
                    break
    
    while n_r <= 0 or n_r=="":
            n_r = int(input("QUAL O NÚMERO DE GANHOS: "))
            if n_r <= 0:
                print("Por favor, insira um valor positivo para o número de ganhos.")
    
    print("INFORME OS GANHOS NOMINAIS ABAIXO: \n")
    for i in range(n_r):
        while True:
                ren = float(input("GANHO %d:"%(i+1)))
                vet_renda.append(ren)
                break

    while sum(vet_renda) != receb:
        print("VALORES INSERIDOS NÃO COINCIDEM COM O VALOR REFERENTE AOS GANHOS BRUTOS.")
        print("TOTAL INFORMADO: R$ %.2f \n" %receb)
        vet_renda.clear()
        for i in range(n_r):
            while True:
                    ren = float(input("GANHO %d: "%(i+1)))
                    vet_renda.append(ren)
                    break
        
    print("ABAIXO AS TABELAS COM OS VALORES NOMINAIS DOS GASTOS E GANHOS NOMINAIS: \n")

    labels_gastos = ["GASTO %d: R$ %.2f "%((i+1),vet_gastos[i]) for i in range(len(vet_gastos))]
    explode_gastos = [0.01 if g == max(vet_gastos) else 0 for g in vet_gastos]
    plt.pie(vet_gastos, labels=labels_gastos, autopct='%1.1f%%', startangle=90, explode=explode_gastos)
    plt.title('DESPESAS MENSAIS (MICROANÁLISE)')
    plt.show()


    labels_renda = ["GANHO %d: R$ %.2f "%((i+1),vet_renda[i]) for i in range(len(vet_renda))]
    explode_renda = [0.01 if r == max(vet_renda) else 0 for r in vet_renda]
    plt.pie(vet_renda, labels=labels_renda, autopct='%1.1f%%', startangle=90, explode=explode_renda)
    plt.title('GANHOS MENSAIS (MICROANÁLISE)')
    plt.show()

    maior_desp = max(vet_gastos)
    menor_desp = min(vet_gastos)
    maior_gan = max(vet_renda)
    menor_gan = min(vet_renda)

    print("A MAIOR DESPESA AFERIDA É DE: R$ %.2f.\n O CORRESPONDE A %.2f%% DA SUA DESPESA BRUTA.  \n"%(maior_desp,(maior_desp / sum(vet_gastos)) * 100))
    print("A MENOR DESPESA AFERIDA É DE: R$ %.2f.\n O CORRESPONDE A %.2f%% DA SUA DESPESA BRUTA. \n"%(menor_desp,(menor_desp / sum(vet_gastos)) * 100))
    print("-" * 40)

    print("O MAIOR GANHO AFERIDO FOI DE: R$ %.2f.\n CORRESPONDE A %.2f%% DO SEU GANHO BRUTO. \n"%(maior_gan,(maior_gan / (sum(vet_renda)) * 100)))
    print("O MENOR GANHO AFERIDO FOI DE: R$ %.2f.\n CORRESPONDE A %.2f%% DO SEU GANHO BRUTO. \n"%(menor_gan,(menor_gan / sum(vet_renda)) * 100))
    print("-" * 40)
       
def Proje(vet_gastos, vet_renda):
    print("GOSTARIA DE PROJETAR SOBRE:  \n")
    print("1-A REDUÇÃO PERCENTUAL DA MAIOR DESPESA \n")
    print("2-O AUMENTO PERCENTUAL DO MENOR GANHO \n")
    print("0- NÃO TENHO INTERESSE \n")
    
    resp = int(input("INDIQUE SUA RESPOSTA AQUI:  "))
    
    if resp == 1:
        resp1 = float(input("INSIRA QUANTO VOÇÊ DESEJA REDUZIR SUA MAIOR DESPESA:  \t  O VALOR SERÁ CONVERTIDO PARA A PORCENTAGEM EQUIVALENTE \n"))
        maior_d = max(vet_gastos) - (max(vet_gastos) * (resp1 / 100))
        pesq = vet_gastos.index(max(vet_gastos))
        vet_gastos.pop(pesq)
        vet_gastos.append(maior_d)
        
        labels_gastos = ["GASTO %d" % (j + 1) for j in range(len(vet_gastos))]
        explode = [0.001 if i == vet_gastos.index(maior_d) else 0 for i in range(len(vet_gastos))]
        plt.pie(vet_gastos, labels=labels_gastos, autopct='%1.1f%%', startangle=90, explode=explode)
        plt.title('PROJEÇÃO DAS NOVAS DESPESAS (MICROANÁLISE)')
        plt.show()
        
        print("NESSAS CIRCUNSTÂNCIAS, A MAIOR DESPESA AFERIDA FOI REDUZIDA PARA : R$ %.2f .\n O QUE CORRESPONDE, NESSA PROJEÇÃO, À %.2f%%  DA SUA DESPESA BRUTA" % (maior_d, (maior_d / sum(vet_gastos) * 100)))
        an=str(input("GOSTARIA DE CONTINUAR NESSA FUNCIONALIDADE?: "))
        if an=="s" or an=="S":
            Proje(vet_gastos, vet_renda)
            
            return
    elif resp == 2:
        resp3 = float(input("INSIRA QUANTO VOÇÊ DESEJA AUMENTAR DO SEU MENOR GANHO:  \t O VALOR SERÁ CONVERTIDO PARA A PORCENTAGEM EQUIVALENTE \n"))
        menor_ganho = (min(vet_renda) * (resp3 / 100)) + min(vet_renda)
        if menor_ganho==0:
            for i, e in enumerate(len(vet_gastos)):
                if e[i]!=0 and e[i]<max(vet_gastos) and e[i]<e[i+1]:
                    menor_ganho = (e[i] * (resp3 / 100)) + e[i]
                    pesq = vet_renda.index(e[i])
                    vet_renda.pop(pesq)
                    vet_renda.append(menor_ganho)
        else:
            pesq = vet_renda.index(min(vet_renda))
            vet_renda.pop(pesq)
            vet_renda.append(menor_ganho)
        
        labels_renda = ["GANHO %d" % (h + 1) for h in range(len(vet_renda))]
        explode = [0.001 if i == vet_renda.index(menor_ganho) else 0 for i in range(len(vet_renda))]
        plt.pie(vet_renda, labels=labels_renda, autopct='%1.1f%%', startangle=90, explode=explode)
        plt.title('PROJEÇÃO DOS NOVOS GANHOS (MICROANÁLISE)')
        plt.show()
        
        print("NESSE CASO, O MENOR GANHO AFERIDO FOI AUMENTADO PARA : R$ %.2f .\n O QUE CORRESPONDE À %.2f%% DO SEU GANHO BRUTO, NESSA PROJEÇÃO" % (menor_ganho, (menor_ganho / sum(vet_renda) * 100)))
        an=str(input("GOSTARIA DE CONTINUAR NESSA FUNCIONALIDADE?: "))
        if an=="s" or an=="S":
            Proje(vet_gastos, vet_renda)
        else:
            return
        
    elif resp == 0:
        print("OBRIGADO PELA ESCOLHA. \nVOLTE SEMPRE QUE PRECISAR DE CONSULTAS E CONTINUE CONOSCO")
        return 0

        
def listar(op, vet, n):
    if op == 1:
        for i in range(n):
             print("GASTO %d: %.2f \n"%(i, vet[i]))
             print("-" * 40)
    else:
        for j in range(n):
            print("GASTO %d: %.2f \n" %(i,vet[j]))
            print("-" * 40)

def atualização(op, l, vet_gastos, vet_renda, n_d, n_r):
    if l == 0:
        deletar(vet_gastos, op)
        listar(1, vet_gastos, n_d)
    else:
        deletar(vet_renda, op)
        listar(2, vet_renda, n_r)

def red(l, op, vet, n):
    if l == 0:
        op1 = int(input("INDIQUE QUAL O PERCENTUAL EQUIVALENTE AO GASTO QUE DESEJA REDUZIR: \n"))
        op1 = op1 / 100
        for i in range(n):
            if vet[i] == vet[op]:
                vet[i] = vet[i] - (vet[i] * op1)
        listar(1, vet, n)
    else:
        op1 = int(input("INDIQUE QUAL O PERCENTUAL EQUIVALENTE AO GANHO QUE DESEJA AUMENTAR: \n"))
        op1 = op1 / 100
        for i in range(n):
            if vet[i] == vet[op]:
                vet[i] = vet[i] + (vet[i] * op1)
        listar(2, vet, n)

def atualizar(vet_gastos, vet_renda, n_d, n_r):
    print("GOSTARIA DE ATUALIZAR ALGUNS DE SEUS DESPESAS OU GANHOS?: \n")
    resp = input("INDIQUE SUA RESPOSTA AQUI: \n")
    
    if resp == "s" or resp=="S":
        print("GOSTARIA DE ATUALIZAR QUAL DAS OPÇÕES?: \n")
        print("1-DESPESAS \n")
        print("2-GANHOS \n")
        resp1 = int(input("INDIQUE AQUI SUA RESPOSTA: \n"))
        
        if resp1 == 1:
            listar(1, vet_gastos, n_d)
            l = 0
        else:
            listar(2, vet_renda, n_r)
            l = 1
        
        print("O QUE DESEJA FAZER?: ")
        print("1-EXCLUIR \n")
        print("2-REDUZIR \n")
        print("3-AUMENTAR \n")
        print("0-VOLTAR \n")
        resp2 = int(input("INDIQUE AQUI SUA RESPOSTA: \n"))
        
        if resp2 == 1:
            op = int(input("INDIQUE O GASTO OU GANHO A EXCLUIR: \n"))
            atualização(op, l, vet_gastos, vet_renda, n_d, n_r)
        elif resp2 == 2:
            op = int(input("INDIQUE QUAL O NÚMERO EQUIVALENTE AO GASTO QUE DESEJA REDUZIR: \n"))
            red(0, op, vet_gastos, n_d)
        elif resp2 == 3:
            op = int(input("INDIQUE QUAL O NÚMERO EQUIVALENTE AO GANHO QUE DESEJA AUMENTAR: \n"))
            red(1, op, vet_renda, n_r)
        else:
            print('OBRIGADO PELA ESCOLHA. ESTAMOS À DISPOSIÇÃO PARA AJUDÁ-LO SEMPRE \n')
            
            
def pesqui(vet, pesq):
    for i, e in enumerate(vet):
        if e == pesq:
            return i
    return -1

def deletar(vet, pesq):
    index = pesqui(vet, pesq)
    if index != -1:
        del vet[index]
        return index


def listar2(vet_cad,op,vet,n):
    if op == 1:
        for i in range(n):
            for h in range (len(vet_cad)):
             print(vet_cad[h])
             print("GASTO %d: %.2f \n"%(i, vet[i]))
             print("-" * 40)
    else:
        for j in range(n):
            for h in range (len(vet_cad)):
             print(vet_cad[h])
            print("GASTO %d: %.2f \n" %(i,vet[j]))
            print("-" * 40)
###### PRINCIPAL ##########
def main():
    vet_gastos = []
    vet_renda = []
    vet_cad = [] 
    vetc=0 
    val=0
    receb=0
    n_div=0
    n_d=0
    n_r=0
    quant=0
    op=-1
    while op != 0:
        op = menu()
        if op==0:
            print("OBRIGADO PELA PREFERÊNCIA E TENHA UM ÓTIMO DIA")
            print("ESTAMOS SEMPRE DISPONÍVEIS PARA OFERECER O MELHOR DE NÓS")

        elif op == 1:
             vet_cad=[]
             vetc=cadastro(vet_cad, quant)
             print("Processo de adezão finalizado")

            # Chamar a função para Inserir os dados nos vetores

        elif op == 2:
            if (vet_cad)!=0:
             vet_div= []
             finanças( vet_div,n_div)
            # Ler a informação para pesquisar
            # Chamar a função para pesquisar no vetor
            # Imprimir os dados se encontrar

        elif op == 3:
            valores(val, receb,vet_gastos,vet_renda,n_d,n_r)
            Proje(vet_gastos,vet_renda)
            
            # Ler a informação para pesquisar
            # Chamar a função para pesquisar no vetor
            # Ler os novos dados
            # Se encontrar, então atualizar o vetor, na posição pesquisada,
            #    com os novos dado


        elif op == 4:
            atualizar(vet_gastos, vet_renda, len(vet_gastos), len(vet_renda)) 
            print("\n\nATUALIZAR\n\n")
            # Listar todos os dados dos vetores
            
        elif op==5:
            print("QUAL OPÇÃO O SENHOR(A) DESEJA LISTAR?: \n")
            print("1-GANHOS")
            print("2-DESPESAS")
            ops= print(int (input("INDIQUE AQUI SUA ESCOLHA: ")))
            if ops==1:
                listar2(vetc,op,vet_renda,)
            else:
               listar2(vetc,op, vet_gastos,)
               print("\n\nLISTAR\n\n")
               print("Processo de listagem finalizado.")
            print("\n\nLISTAR\n\n")

        else:
            print("Opção inválida!")

if __name__ == "__main__":
    main()

```
