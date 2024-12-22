# Projet-Antibio-Tech---Chirine-et-Chiara

#import libraries
import os
import pandas as pd
import matplotlib.pyplot as plt

#print error if input file not found 
if not os.path.exists('./input/data_real.csv'):
    raise FileNotFoundError("File not found in the folder 'input'.")

# read csv data file with separator ';' and import into dataframe
data_input = pd.read_csv('./input/data_real.csv', sep=';') 
print("Input data loaded with success.")
print(data_input)

# Create folders if necessary
os.makedirs('output', exist_ok=True)
os.makedirs('images', exist_ok=True)

# Graph 1:
# => line graph of number of bacteria per mouse and per day

# get list of mouse ID 
list_of_mouses=data_input['mouse_ID'].unique()

# for each mouse in list, loop to plot
for id in list_of_mouses:
        # filter dataframe to get data from mouse ID and fecal with ABX
        df_fe_abx = data_input[(data_input['sample_type']=='fecal')&(data_input['mouse_ID']==id)&(data_input['treatment']=='ABX')] 
        
        # filter dataframe to get data from mouse ID and fecal with placebo
        df_fe_pla = data_input[(data_input['sample_type']=='fecal')&(data_input['mouse_ID']==id)&(data_input['treatment']=='placebo')] 
        
        # plot datas abx in red, placebo in blue
        plt.plot(df_fe_abx['experimental_day'],df_fe_abx['counts_live_bacteria_per_wet_g'], color = 'orange', linewidth = '0.5')
        plt.plot(df_fe_pla['experimental_day'],df_fe_pla['counts_live_bacteria_per_wet_g'], color = 'blue', linewidth = '0.5')

# use logarithm vertical axis and add labels to axis
plt.yscale('log')
plt.xlabel('Washout day')
plt.ylabel('live bacteria /wet g')
plt.title("Fecal live bacteria")

# plotting 2 dummy points to add labels to series
plt.plot([0],[0], color = 'orange', linewidth = '0.5',label = 'ABX')
plt.plot(0,0, color = 'blue', linewidth = '0.5',label = 'Placebo')
plt.legend()

# save line graph
plt.savefig('./images/graph_lines.png')
print("Line graph of number of bacteria per mouse and per day saved in the folder 'images'.")

# save ouput data for graph 1
data_output=data_input[data_input['sample_type']=='fecal']
data_output.to_csv('./output/output_file_fecal.csv',header=True,index=False, sep=';',columns=['mouse_ID','experimental_day','counts_live_bacteria_per_wet_g'])
print("Output data saved for line graph in output_file_fecal.csv.")  




# Graph 2:
# => violin graph of bacteria in cecal samples

# filter dataframe for cecal
df_cec_abx = data_input[(data_input['sample_type']=='cecal')&(data_input['treatment']=='ABX')] 
df_cec_pla = data_input[(data_input['sample_type']=='cecal')&(data_input['treatment']=='placebo')] 

# prepare figure geometry for cecal
figc, axc = plt.subplots(figsize=(8, 6)) 

# plot the data for abx
vp_abx = axc.violinplot(df_cec_abx['counts_live_bacteria_per_wet_g'],positions=[1])
for body in vp_abx['bodies']:
    body.set_facecolor('orange') #define color
    body.set_alpha(0.7)

# plot the data for placebo
vp_pla = axc.violinplot(df_cec_pla['counts_live_bacteria_per_wet_g'],positions=[2])
for body in vp_pla['bodies']:
    body.set_facecolor('blue') #define color
    body.set_alpha(0.7)
    
# add titles and use log scale
axc.set_title('Cecal live bacteria')
axc.set_yscale('log')
axc.set_xticks([1, 2])
axc.set_xticklabels(['ABX', 'Placebo'])
axc.set_xlabel('Treatment')
axc.set_ylabel('live bacteria/wet g')


# save graph
plt.savefig('./images/graph_cecal.png')
print("Violin graph of bacteria in cecal samples saved in the folder 'images'.")

# save ouput data for graph 2  
data_output=data_input[(data_input['sample_type']=='cecal')]
data_output.to_csv('./output/output_file_cecal.csv',header=True,index=False, sep=';',columns=['mouse_ID','sample_type','counts_live_bacteria_per_wet_g'])
print("Output data saved for violin graph 1 in output_file_cecal.csv.")


# Graph 3:
# => violin graph of bacteria in ileal samples

# filter dataframe for ileal
df_ile_abx = data_input[(data_input['sample_type']=='ileal')&(data_input['treatment']=='ABX')] 
df_ile_pla = data_input[(data_input['sample_type']=='ileal')&(data_input['treatment']=='placebo')] 

# prepare figure geometry for ileal 
figi, axi = plt.subplots(figsize=(8, 6)) 

# plot the data for ABX
vp_abx = axi.violinplot(df_ile_abx['counts_live_bacteria_per_wet_g'],positions=[1])
for body in vp_abx['bodies']:
    body.set_facecolor('orange') #define color
    body.set_alpha(0.7)

# plot the data for placebo
vp_pla = axi.violinplot(df_ile_pla['counts_live_bacteria_per_wet_g'],positions=[2])
for body in vp_pla['bodies']:
    body.set_facecolor('blue') #define color
    body.set_alpha(0.7)
    
# add title and use log scale
axi.set_title('Ileal live bacteria')
axi.set_yscale('log')
axi.set_xticks([1, 2])
axi.set_xticklabels(['ABX', 'Placebo'])
axi.set_xlabel('Treatment')
axi.set_ylabel('live bacteria/wet g')

# save graph
plt.savefig('./images/graph_ileal.png')
print("Violin graph of bacteria in ileal samples saved in the folder 'images'.")
  

# save ouput data for graph 3
data_output=data_input[data_input['sample_type']=='ileal']
data_output.to_csv('./output/output_file_ileal.csv',header=True,index=False, sep=';',columns=['mouse_ID','sample_type','counts_live_bacteria_per_wet_g'])
print("Output data saved for violin graph 2 in output_file_ileal.csv.")




