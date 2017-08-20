import requests
import bs4
import time

def regcrawl(region):
    def cut(reg_raw):
        start=reg_raw.find('<div class="main__content ">')
        end=reg_raw.find(' <div id="gls-subscribe">')
        reg_raw=reg_raw[start:end]
        regbs=bs4.BeautifulSoup(reg_raw)
        return regbs
    
    reg_raw=requests.get('https://www.golosinfo.org'+region.get('href')).text
    regbs=cut(reg_raw)
    flag=regbs.findAll(lambda tag: tag.name=='span' and tag.get('class')==["page"])
    if flag==[]:
        count=len(regbs.findAll('b'))+len(regbs.findAll('h1'))
    else:
        reg_raw=requests.get('https://www.golosinfo.org'+region.get('href')+'?page='+str(len(flag)+1)).text
        regbs=cut(reg_raw)
        count=25*len(flag)+len(regbs.findAll('b'))+len(regbs.findAll('h1'))
    return(count)
    
    

region_raw=requests.get('https://www.golosinfo.org/ru/regions/').text
start=region_raw.find('<h3>Регионы</h3>')
end=region_raw.find('<gls-subscribe></gls-subscribe>')
region_raw=region_raw[start:end]

regionbs=bs4.BeautifulSoup(region_raw)

reglist=regionbs.findAll('a')

filefin=open('golos.csv', "w")
filefin.write('Region, News \n')

for region in reglist:
    filefin.write(region.text+',')
    print(region.text+',')
    try:
        count=regcrawl(region)
    except requests.exceptions.ConnectionError:
        time.sleep(10)
        count=regcrawl(region)
    filefin.write(str(count)+'\n')
    print(count)

filefin.close()
