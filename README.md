import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

df = pd.read_csv("StudentsPerformance.csv")

df[['Sem1','Sem2','Sem3']] = df[['math score','reading score','writing score']]
df['attendance'] = np.random.randint(60,100,len(df))
df['avg'] = df[['Sem1','Sem2','Sem3']].mean(axis=1)
df['consistency'] = df[['Sem1','Sem2','Sem3']].std(axis=1)

df['risk'] = np.where((df['avg']<50)|(df['attendance']<70),'High',
              np.where(df['avg']<65,'Medium','Low'))

difficulty = 100 - df[['Sem1','Sem2','Sem3']].mean()
print("Attendance–Marks Correlation:", df['attendance'].corr(df['avg']))

for i in range(5):
    plt.plot(['Sem1','Sem2','Sem3'], df.loc[i,['Sem1','Sem2','Sem3']])
plt.title("Performance Progression"); plt.show()

sns.heatmap(df[['Sem1','Sem2','Sem3']].corr(), annot=True)
plt.title("Subject Heatmap"); plt.show()

sns.boxplot(data=df[['Sem1','Sem2','Sem3']])
plt.title("Score Distribution"); plt.show()

plt.scatter(df['attendance'], df['avg'])
plt.xlabel("Attendance"); plt.ylabel("Average Score")
plt.title("Attendance vs Performance"); plt.show()

df['risk'].value_counts().plot(kind='bar')
plt.title("Dropout Risk Levels"); plt.show()
