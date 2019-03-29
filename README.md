# Coursera
def answer_one():
    def get_energy():
        energy = pd.read_excel('Energy Indicators.xls',  skip_footer=38) #read the file
        energy=energy.drop([0,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15])
        energy= energy.drop(["Unnamed: 0",'Environmental Indicators: Energy'],axis=1) #drop columns
        energy.columns=['Country', 'Energy Supply', 'Energy Supply per Capita', '% Renewable'] #rename headers
        
        energy=energy.replace('...',np.NaN) #replace missing value to NaN
       
        energy['Energy Supply']*=1000000 #convert to whatever they are asking to conver
       
        energy['Country'].replace({"Republic of Korea": "South Korea",
"United States of America": "United States",
"United Kingdom of Great Britain and Northern Ireland": "United Kingdom",
"China, Hong Kong Special Administrative Region": "Hong Kong"}, inplace=True)
        energy['Country'] = energy['Country'].str.replace(r" \(.*\)","")
        energy.set_index('Country',inplace=True)
        return energy
    
    
    def get_gdp():

        gdp = pd.read_csv('world_bank.csv',skiprows=4)
        gdp['Country Name'] = gdp['Country Name'].replace({'Korea, Rep.':'South Korea','Iran, Islamic Rep.':'Iran','Hong Kong SAR, China':'Hong Kong'})
        gdp = gdp[['Country Name','2006','2007','2008','2009','2010','2011','2012','2013','2014','2015']]
        gdp.columns = ['Country','2006','2007','2008','2009','2010','2011','2012','2013','2014','2015']
        gdp.set_index("Country", inplace=True)
        return gdp
    
    def get_scimen():
        ScimEn = pd.read_excel('scimagojr-3.xlsx')
     
        ScimEn.set_index('Country', inplace=True)
        return ScimEn
    
    energy, gdp, scimen = get_energy(), get_gdp(), get_scimen()

    scimen=scimen[scimen["Rank"]<=15]
    d1=scimen.merge(energy, how='inner',left_index = True, right_index = True)
    d2=d1.merge(gdp, how='inner',left_index = True, right_index = True)
   
    d2.columns=  ['Rank', 'Documents', 'Citable documents', 'Citations', 'Self-citations', 'Citations per document', 'H index', 'Energy Supply', 'Energy Supply per Capita', '% Renewable', '2006', '2007', '2008', '2009', '2010', '2011', '2012', '2013', '2014', '2015']
    d2 = d2.sort_values(by = "Rank")
    return d2
answer_one()
