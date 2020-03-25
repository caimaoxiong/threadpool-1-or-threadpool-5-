# -*-coding:utf8-*-
from __future__ import print_function
import requests
from bs4 import BeautifulSoup
import threading
#from multiprocessing import Pool
from multiprocessing.pool import ThreadPool
#from threading import Pool
import time
proxy_ips=[]
header={'User-Agent':'Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/535.1 (KHTML, like Gecko) Chrome/14.0.835.163 Safari/535.1'}
proxy_ips_url=r'./ips0.txt'
def get_ips(i):
    url=r'http://www.xiladaili.com/gaoni/'+str(i)+'/'
    print(url)
    resp=requests.get(url,headers=header)
    soup=BeautifulSoup(resp.text,'html')
    ips_tr=soup.find_all('tr')
    for ip_tr in ips_tr:
        if ip_tr.find_all('td'):
            ip=ip_tr.find_all('td')[0].text
            print(ip)
            proxy_ips.append(ip)
            

def multi_processing():
    time1=time.time()
    pool = ThreadPool(5)  #注意这里
    pool.map(get_ips,[x for x in range(1,11)])
    print("proxy_ips", proxy_ips)
    with open("multi_ip.txt",'w') as f:
        for ip in proxy_ips:
            f.write(ip+'\n')
    time2=time.time()
    print(time1-time2)    
    
def single_prcessing():
    for i in range(10):        
        get_ips(i) 
    with open("single_ip.txt",'a') as f:
        for ip in proxy_ips:
            f.write(ip+'\n')    
    
if __name__ =='__main__':
    #single_prcessing()
    multi_processing()
    
    

                

