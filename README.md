# Trabalho-mat
```
def menu():
    print("\nNuBolso:")
    print("1 - CADASTRO")
    print("2 - FINANÇAS (SALDO MENSAL E MACROANÁLISE DE DADOS)")
    print("3 - PROJEÇÕES (MICROANÁLISE DE GANHOS E DESPESAS)")
    print("4 - REAJUSTE DE FINANÇAS (EXCLUSÃO, AUMENTO E REDUÇÃO DE DESPESAS E GANHOS)")
    print("5 - LISTAGEM")
    print("6 - IMPOSTO DE RENDA(IR)")
    print("7 -IMPOSTO SOBRE BENS") 
    print("8 - FISCALIZAÇÃO DO CARTÃO DE CRÉDITO")
    print("9 - PROJEÇÃO APLICADA")
    print("0 - SAIR")
    op = input("INFORME SUA DECISÃO: ")
    while not op.isdigit() or int(op) not in range(0, 10):
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
   
    for i in range(12 - (mes-1)):
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

def graf2(vet):
    labels_gastos = ["GASTO %d: R$ %.2f "%((i+1),vet[i]) for i in range(len(vet))]
    explode_gastos = [0.01 if g == max(vet) else 0 for g in vet]
    plt.pie(vet, labels=labels_gastos, autopct='%1.1f%%', startangle=90, explode=explode_gastos)
    plt.title('DESPESAS MENSAIS (MICROANÁLISE)')
    plt.show()


    labels_renda = ["GANHO %d: R$ %.2f "%((i+1),vet[i]) for i in range(len(vet))]
    explode_renda = [0.01 if r == max(vet) else 0 for r in vet]
    plt.pie(vet, labels=labels_renda, autopct='%1.1f%%', startangle=90, explode=explode_renda)
    plt.title('GANHOS MENSAIS (MICROANÁLISE)')
    plt.show()

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

    graf2(vet_gastos)
    graf2(vet_renda)

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
    
    return menu()

def graf3(i,vet, m):
    if i=="g":
        labels_gastos = ["GASTO %d" % (j + 1) for j in range(len(vet))]
        explode = [0.001 if i == vet.index(m) else 0 for i in range(len(vet))]
        plt.pie(vet, labels=labels_gastos, autopct='%1.1f%%', startangle=90, explode=explode)
        plt.title('PROJEÇÃO DAS NOVAS DESPESAS (MICROANÁLISE)')
        plt.show()
    
    else:
        labels_renda = ["GANHO %d" % (h + 1) for h in range(len(vet))]
        explode = [0.001 if i == vet.index(m) else 0 for i in range(len(vet))]
        plt.pie(vet, labels=labels_renda, autopct='%1.1f%%', startangle=90, explode=explode)
        plt.title('PROJEÇÃO DOS NOVOS GANHOS (MICROANÁLISE)')
        plt.show()

    
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
        
        graf3("g",vet_gastos,maior_d)
        
        print("NESSAS CIRCUNSTÂNCIAS, A MAIOR DESPESA AFERIDA FOI REDUZIDA PARA : R$ %.2f .\n O QUE CORRESPONDE, NESSA PROJEÇÃO, À %.2f%%  DA SUA DESPESA BRUTA" % (maior_d, (maior_d / sum(vet_gastos) * 100)))
        an=str(input("GOSTARIA DE CONTINUAR NESSA FUNCIONALIDADE?: "))
        if an=="s" or an=="S":
            Proje(vet_gastos, vet_renda)

    elif resp == 2:
        resp3 = float(input("INSIRA QUANTO VOÇÊ DESEJA AUMENTAR DO SEU MENOR GANHO:  \t O VALOR SERÁ CONVERTIDO PARA A PORCENTAGEM EQUIVALENTE \n"))
        menor_ganho = (min(vet_renda) * (resp3 / 100)) + min(vet_renda)
        if menor_ganho==0:
            for i, e in enumerate(vet_gastos):
                if e[i]!=0 and e[i]<max(vet_gastos) and e[i]<e[i+1]:
                    menor_ganho = (e[i] * (resp3 / 100)) + e[i]
                    pesq = vet_renda.index(e[i])
                    vet_renda.pop(pesq)
                    vet_renda.append(menor_ganho)
        else:
            pesq = vet_renda.index(min(vet_renda))
            vet_renda.pop(pesq)
            vet_renda.append(menor_ganho)
        
        graf3("r",vet_renda,menor_ganho)
      
        print("NESSE CASO, O MENOR GANHO AFERIDO FOI AUMENTADO PARA : R$ %.2f .\n O QUE CORRESPONDE À %.2f%% DO SEU GANHO BRUTO, NESSA PROJEÇÃO" % (menor_ganho, (menor_ganho / sum(vet_renda) * 100)))
        an=str(input("GOSTARIA DE CONTINUAR NESSA FUNCIONALIDADE?: "))
        if an=="s" or an=="S":
            Proje(vet_gastos, vet_renda)
        else:
            return
        
    elif resp == 0:
        print("OBRIGADO PELA ESCOLHA. \nVOLTE SEMPRE QUE PRECISAR DE CONSULTAS E CONTINUE CONOSCO")
        return menu()

        
def listar(op, vet, n):
    if op == 1:
        for i in range(n):
             print("GASTO %d: %.2f \n"%(i, vet[i]))
             print("-" * 40)
    else:
        for j in range(n):
            print("GASTO %d: %.2f \n" %(i,vet[j]))
            print("-" * 40)
            
def listar2(vet_cad, op, vet, n):
    for i in range(n):
        print("USUÁRIO CADASTRADO:")
        for h in range(len(vet_cad)):
            print(vet_cad[h])  
        if op == 1:
            print("GANHO %d: R$ %.2f \n" % (i + 1, vet[i]))  
        else:
            print("GASTO %d: R$ %.2f \n" % (i + 1, vet[i]))
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
            return menu()
                      
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

def imposto1(vet_gastos):
    print("INDIQUE O MES PELO NÚMERO CORRESPONDENTE.\t Ex: 1- JANEIRO \t 2- FEVEREIRO \t 3- MARÇO\t")
    mes = int(input("INDIQUE-O SIMBOLIZANDO QUANDO VC GOSTARIA DE COMEÇAR A PROGRAMAR SUAS FINANÇAS: "))
   
    while mes < 1 or mes > 12:
        print("Mês inválido. Por favor, informe um número entre 1 e 12.")
        mes = int(input("INDIQUE O MÊS O QUAL VC GOSTARIA DE COMEÇAR A PROGRAMAR SUAS FINANÇAS:  \n"))
   
    vet_impost=[]
    anu=str(input("VOÇÊ POSSUI IMPOSTOS ANUAIS A PAGAR?: "))
    men=str(input("VOÇÊ POSSUI IMPOSTOS MENSAIS A PAGAR?: "))
    
    if (anu=="s" or anu =="S") and (men=="s" or men=="S") :
     bem=int(input("QUANTOS BENS VC POSSUI QUE PAGAM IMPOSTOS ANUAIS: "))
     bem2=int(input("QUANTOS BENS VC POSSUI QUE PAGAM IMPOSTOS MENSAIS: "))
     for i in range(bem):
       imp_al=(float(input("ALIQUOTA ANUAL SOBRE O BEM (%%) %d: " % (i+1))))
       imp_val=(float(input("VALOR DO BEM %d: " % (i+1))))
       impost_tot=imp_val*(imp_al/100)
       vet_impost.append(impost_tot)
       
     for j in range(bem2):
       imp_al2=(float(input("ALIQUOTA MENSAL SOBRE O BEM (%%) %d: " % (j+1))))
       imp_val2=(float(input("VALOR DO BEM %d: " % (j+1))))
       valor_tot=((imp_al2/100)*imp_val2)*12    
       vet_impost.append(valor_tot)
       impost_tot=sum(vet_impost)
       receb_to=(sum(vet_gastos)*(12-mes)) + impost_tot 
       print("ANUALMENTE,VOÇÊ TEM GASTOS DE %.2f\nINCLUINDO, IMPOSTOS MENSAIS E GASTOS OBRIGATÓRIOS"% receb_to)
       
    elif (anu=="n" or anu =="N") and (men=="s" or men=="S") :
     bem2=int(input("QUANTOS BENS VC POSSUI QUE PAGAM IMPOSTOS MENSAIS: "))
     for j in range(bem2):
       imp_al2=(float(input("ALIQUOTA MENSAL SOBRE O BEM (%%) %d: " %(j+1))))
       imp_val2=(float(input("VALOR DO BEM %d: " % (j+1))))
       valor_tot=((imp_al2/100)*imp_val2)*12    
       vet_impost.append(valor_tot)
       impost_tot=sum(vet_impost)
       receb_to=(sum(vet_gastos)*(12-mes)) + impost_tot 
       print("MENSALMENTE,VOÇÊ TEM GASTOS DE %.2f\nINCLUINDO, IMPOSTOS MENSAIS E GASTOS OBRIGATÓRIOS"% receb_to)
       
    elif (anu=="s" or anu =="S") and (men=="n" or men=="N"):
     bem=int(input("QUANTOS BENS VC POSSUI QUE PAGAM IMPOSTOS ANUAIS: "))
     for i in range(bem):
       imp_al=(float(input("ALIQUOTA ANUAL SOBRE O BEM  (%%) %d: " % (i+1))))
       imp_val=(float(input("VALOR DO BEM %d: "% (i+1))))
       impost_tot=imp_val*(imp_al/100)
       vet_impost.append(impost_tot)
       impost_tot=sum(vet_impost)
       receb_to=(sum(vet_gastos)*(12-mes)) + impost_tot 
       print("ANUALMENTE,VOÇÊ TEM GASTOS DE %.2f\nINCLUINDO, IMPOSTOS ANUAIS E GASTOS OBRIGATÓRIOS"% receb_to)
    
    else:
        print("OBRIGADO PELA ESCOLHA. ESTAMOS SEMPRE A DISPOSIÇÃO")
        return menu()
    
def juros(gast,jur):
    montante = gast * ((1 + jur) ** 5)
    if jur==0:
        print("\t\tVOÇÊ ESTÁ NA MELHOR SITUAÇÃO POSSÍVEL\nSEUS JUROS NÃO COMPROMETEM SUA RENDE EN NÍVEL COMPROMETEDOR")
        print("*"*40)
    elif jur>=0.01 and jur<=0.02:
        print("\t\tNECESSÁRIO ESTAR EM ALERTA\nAS CONTAS PODEM APERTAR COM ESSES JUROS")
        print("*"*40)
        print("ESSE SERÁ O TOTAL À PAGAR DEPOIS DE 5 MESES SEM PAGAR A FATURA: %.2f"%(montante))
    elif jur>=0.03 and jur<=0.05:
        print("\t\tJUROS ESTÃO ELEVADOS.\nEVITE FALTAR COM O PAGAMENTO EM DIA DA FATURA")
        print("*"*40)
        print("ESSE SERÁ O TOTAL À PAGAR DEPOIS DE 5 MESES SEM PAGAR A FATURA: %.2f"%(montante))
    else:
     print("\t\tESSES JUROS ESTÃO EM NÍVEIS ALTISSÍMOS E PRECISAM SER EVITADOS\nNÃO PAGAR AS FATURAS VAI LHE TRAZER SÉRIOS PROBLEMAS")
     print("ESSE SERÁ O TOTAL À PAGAR DEPOIS DE 5 MESES SEM PAGAR A FATURA: %.2f"%(montante))
    
def cartão():
    an=str(input("VOÇÊ USA CARTÃO DE CRÉDITO COM FREQUÊNCIA?: "))
    if an=="s" or an =="S":
        limi=float(input("INDIQUE O LIMITE DO SEU CARTÃO DE CRÉTIDO: "))
        while limi <= 0:
            limi = float(input("LIMITE INVÁLIDO. INFORME UM VALOR POSITIVO: "))
        gast=float(input("INDIQUE QUANTOS REAIS POR MES VC COSTUMA GASTAR COM CARTÃO DE CRÉDITO: "))
        while gast < 0:
            gast = float(input("GASTO INVÁLIDO. INFORME UM VALOR POSITIVO: "))
        jur=float(input("INDIQUE OS JUROS DESCRITOS NO ACORDO FEITO COM O BANCO(EM %): "))
        while jur < 0:
            jur = float(input("JUROS INVÁLIDOS. INFORME UM VALOR POSITIVO: "))
        juros(gast,jur/100)
        
        sau=gast/limi
        if sau<=0.3:
           print("\n\nPARABÉNS!!!\n\n\VOÇÊ POSSUI UMA RELAÇÃO SAÚDAVEL COM SUAS FINAÇAS NO QUE TANGE AO USO DO CARTÃO DE CREDITO")

        elif sau > 0.3 and sau <= 0.5:
            print("SUAS FINANÇAS APERESENTAM NÍVEL MODERADO DE PERICULOSIDADE AO USAR O CARTÃO DE CRÉDITO.\nASSEGURE-SE DE NÃO ABUSAR DESSA FUNÇÃO")    
        
        elif sau > 0.5 and sau <= 0.7:
            print("SUAS FINAÇAS POSSUEM RISCO DE TRONAREM-SE UM FARDO PARA VOÇÊ\nOS JUROS PODEM SER UM GRANDE INIMIGO NESSA ETAPA")
        
        elif sau > 0.7 and sau <= 0.9:
            print("\n\nATENÇÃO\n\n\ VOÇÊ CORRE GRANDE RISCO DE SE ENDIVIDAR E NÃO CONSEGUIR RECUPERAR A ESTABILIDADE\nPARE AGORA DE USAR O CARTÃO DE CRÉDITO")
        
        else:
            print("VOÇÊ GASTA MUITO ACIMA DO NÍVEL SAÚAVEL E/OU MODERADO\nSUAS FINANÇAS POSSUEM CHANCES MINÚSCULAS DE SAÚDE OU ESTABILIDADE")
    else:
        print("OBRIGADO PELA ESCOLHA. ESTAMOS SEMPRE DISPONÍEIS PRA VOÇÊ")
        return menu()       
    

def proj_aplicada():
    val = 0
    receb = 0
    meses = ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec']
    
    print("INDIQUE O MÊS PELO NÚMERO CORRESPONDENTE.\t Ex: 1- JANEIRO \t 2- FEVEREIRO \t 3- MARÇO\t")
    mes = int(input("INDIQUE-O SIMBOLIZANDO QUANDO VC GOSTARIA DE COMEÇAR A PROGRAMAR SUAS FINANÇAS: "))
    
    while val <= 0:
        val = float(input("INFORME O TOTAL DAS SUAS DESPESAS PADRÕES (EM R$): "))
        if val <= 0:
            print("Valor inválido. Você precisa informar uma fonte de despesas acima de 0.")
    
    while receb <= 0:  
        receb = float(input("INFORME SEUS GANHOS TOTAIS PADRÕES (EM R$): "))
        if receb <= 0:
            print("Valor inválido. Você precisa informar uma fonte de ganhos acima de 0.")
            
    sal = receb - val  
    
    while mes < 1 or mes > 12:
        print("Mês inválido. Por favor, informe um número entre 1 e 12.")
        mes = int(input("INDIQUE O MÊS O QUAL VC GOSTARIA DE COMEÇAR A PROGRAMAR SUAS FINANÇAS:  \n"))
   
    taxa = float(input("INFORME A TAXA DE INFLAÇÃO DO SEU PAÍS: "))
    saldos_exponenciais = []
    
   
    for i in range(mes - 1, 12): 
        if sal > 0:
            saldo = sal * ((1 - (taxa / 100)) ** (i - (mes - 1)))  
        else:
            saldo = sal * ((1 + (taxa / 100)) ** (i - (mes - 1)))  
        saldos_exponenciais.append(saldo)

    ponto_critico = None
    for i, saldo in enumerate(saldos_exponenciais):
        if saldo < 0:
            ponto_critico = i
            break
    
    plt.figure(figsize=(10, 6))
    plt.plot(meses[mes-1:], saldos_exponenciais, label="Saldo (Exponencial)", color="blue", marker="o")
    if ponto_critico is not None:
        plt.axvline(x=ponto_critico + (mes - 1), color="orange", linestyle="--", label=("Ponto Crítico (Mês %d)" % (ponto_critico + mes)))
    plt.title("Impacto da Inflação no Saldo (Curva Exponencial)")
    plt.xlabel("Meses")
    plt.ylabel("Saldo (R$)")
    plt.grid(True, linestyle="--", alpha=0.7)
    plt.legend()
    plt.show()


def calcula_imposto_renda(renda):
    if renda <= 2259.20:
        return 0 
    elif renda >= 2259.20 and renda <= 2826.65:
        aliquota = 0.075
        deducao = 169.44
    elif renda >= 2826.65 and renda <= 3751.05:
        aliquota = 0.15
        deducao = 381.44
    elif renda >= 3751.05 and renda <= 4664.68:
        aliquota = 0.225
        deducao = 662.77
    else:
        aliquota = 0.275
        deducao = 896.00

    imposto = (renda * aliquota) - deducao
    return max(0, imposto)

    
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

        elif op == 2:
            if (vet_cad)!=0:
             vet_div= []
             finanças( vet_div,n_div)
    
        elif op == 3:
            valores(val, receb,vet_gastos,vet_renda,n_d,n_r)
            Proje(vet_gastos,vet_renda)
        

        elif op == 4:
            atualizar(vet_gastos, vet_renda, len(vet_gastos), len(vet_renda)) 
            print("\n\nATUALIZAR\n\n")
            
        elif op==5:
            print("QUAL OPÇÃO O SENHOR(A) DESEJA LISTAR?: \n")
            print("1-GANHOS")
            print("2-DESPESAS")
            ops= print(int (input("INDIQUE AQUI SUA ESCOLHA: ")))
            if ops==1:
                listar2(vetc,ops,vet_renda,len(vet_renda))
            else:
               listar2(vetc,ops, vet_gastos,len(vet_gastos))
            print("Processo de listagem finalizado.")
            
        elif op == 6: 
         renda = float(input("Informe sua renda mensal (em R$): "))
         imposto = calcula_imposto_renda(renda)
         print("O VALOR NECESSÁRIO A PAGAR , POR MES , É DE : R$ %.2f " % imposto)
        
        elif op==7:
          imposto1(vet_gastos)
        
        elif op==8:
            cartão()
        
        elif op==9:
            proj_aplicada()

        else:
            print("Opção inválida!")

if __name__ == "__main__":
    main()


```
