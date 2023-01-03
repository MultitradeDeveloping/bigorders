#pip install binance-connector

from binance.client import Client 
from datetime import datetime, timezone
import pandas as pd


client = Client('', '', testnet=False)

cf = input('В сколько раз ордер должен превышать средний объём за минуту? ')
cf = float(cf)

def start():
    x = client.futures_symbol_ticker()
    x = pd.DataFrame(x)
    #Quartal Futures
    x = x[~((x.symbol.str.contains('_'))) ]
    #HNT (incorrectly API-info)
    x = x[~((x.symbol.str.contains('HNTUSDT'))) ]
    #Indexes (soon)
    x = x[~((x.symbol.str.contains('FOOTBALL'))) ]
    x = x[~((x.symbol.str.contains('BTCDOM'))) ]
    x = x[~((x.symbol.str.contains('BLUEBIRD'))) ]
    x = x[~((x.symbol.str.contains('DEFIUSDT'))) ]
    x = x['symbol']
    xs = x.count()
    i = 0
    x = x[::1].reset_index(drop=True)
    print('программа работает')
    while i <= xs-1:
        symb = x[i]
        getavg(symb)
        i = i+1
        
    print('все пары проверены')


def getavg(symb):

symb = str(symb)
    k = 0
    av = client.futures_klines(symbol = symb, interval='1m')
    av = pd.DataFrame(av)
    av = av[5]
    avv = 0
    for i in range(500):
        pl = av.iloc[i]
        pl = float(pl) 
        avv = avv+pl
    avv = avv/1000

    if(symb.__contains__('1000')):
        k = 1
        symb = symb.replace('1000', '')
        avv = avv*1000
    if(symb.__contains__('LUNA2')):
        k = 2
        symb = symb.replace('LUNA2', 'LUNA')
    if(avv!=0):
        spotorder(avv, symb, k)


def spotorder(avv, symb, k):
    symb = str(symb)
    ob = client.get_order_book(symbol = symb)
    ob = str(ob)
    c = ob.count('.')
    c = c/4
    if(c>500):
        r = 500
        ob = client.get_order_book(symbol = symb, limit = r)
    elif(c>100):
        r = 100
        ob = client.get_order_book(symbol = symb, limit = r)
    elif(c>20):
        r = 20
        ob = client.get_order_book(symbol = symb, limit = r)
    else:
        r = 5
        ob = client.get_order_book(symbol = symb, limit = r)
    ob = pd.DataFrame(ob)

    #bids management
    for i in range(r):
    # converting
        b = pd.DataFrame({"bids":[ob['bids'].iloc[i]]})
        bstr = str(b['bids'])
        bstr = str(bstr)
        
    # removing brackets
        bb1 = bstr.find('[')
        bb2 = bstr.find(']')
        bstr = bstr[bb1+1: bb2-1:]
        
    # price and lots separating
        bb = bstr.find(', ')
        bval = bstr[bb+2:]
        bprc = bstr[:bb-1]
        
        if(bval == ''):
            continue
    # converting data in float format
        bval = float(bval)
        bprc = float(bprc)
        
        if(bval > avv*cf):
            print(symb)
            print('SPOT bid ' + str(bprc) + ' - ' + str(bval))
        
    
    #ask management
        a = pd.DataFrame({"asks":[ob['asks'].iloc[i]]})
        astr = str(a['asks'])
        astr = str(astr)
    # removing brackets
        ab1 = astr.find('[')
        ab2 = astr.find(']')
        astr = astr[ab1+1: ab2-1:]
    # price and lots separating
        ab = astr.find(', ')
        aval = astr[ab+2:]
        aprc = astr[:ab-1]
        if(aval == ''):
            continue
    # converting data in float format
        aval = float(aval)
        aprc = float(aprc)
        if(aval > avv*cf):
            print(symb)
            print('SPOT ask ' + str(aprc) + ' - ' + str(aval))
    if(k == 1):
        symb = '1000' + symb
        
    elif(k == 2):
        symb = symb.replace('LUNA', 'LUNA2')
    
    if(avv!=0):
        futorder(avv, symb)

def futorder(avv, symb):
    ob = client.futures_order_book(symbol = symb)
    ob = str(ob)
    c = ob.count(',')
    c = c/4
    r = 500
    if(c>=500):
        r = 500
        ob = client.futures_order_book(symbol = symb, limit = r)
    elif(c>=100):
        r = 100
        ob = client.futures_order_book(symbol = symb, limit = r)
    elif(c>=20):
        r = 20
        ob = client.futures_order_book(symbol = symb, limit = r)
    else:
        r = 5
        ob = client.futures_order_book(symbol = symb, limit = r)

    ob = pd.DataFrame(ob) 
    #bids management
    for i in range(r):
    # converting
        b = pd.DataFrame({"bids":[ob['bids'].iloc[i]]})
        bstr = str(b['bids'])
        bstr = str(bstr)
    # removing brackets
        bb1 = bstr.find('[')
        bb2 = bstr.find(']')
        bstr = bstr[bb1+1: bb2-1:]
    # price and lots separating
        bb = bstr.find(', ')
        bval = bstr[bb+2:]
        bprc = bstr[:bb-1]
        if(bval == ''):
            continue
    # converting data in float format
        bval = float(bval)
        bprc = float(bprc)
        axax = avv*cf
        if(bval > avv*cf):
            print(symb)
            print('FUTURES bid ' + str(bprc) + ' - ' + str(bval))
        
    #ask management
        a = pd.DataFrame({"asks":[ob['asks'].iloc[i]]})
        astr = str(a['asks'])
        astr = str(astr)
    # removing brackets
        ab1 = astr.find('[')
        ab2 = astr.find(']')
        astr = astr[ab1+1: ab2-1:]
    # price and lots separating
        ab = astr.find(', ')
        aval = astr[ab+2:]
        aprc = astr[:ab-1]
        if(aval == ''):
            continue
    # converting data in float format
        aval = float(aval)
        aprc = float(aprc)
        axax = avv*cf
        if(aval > axax):
            print(symb)
            axaxa = aval / avv
            print('FUTURES ask ' + str(aprc) + ' - ' + str(aval))


start()
