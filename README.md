import pandas as pd
import matplotlib.pyplot as plt

df = pd.read_csv("StudentsPerformance.csv")

print("Dataset Preview:\n", df.head())
print("\nShape:", df.shape)
print("\nColumns:", df.columns)

print("\nScore Statistics:")
print(df[['math score', 'reading score', 'writing score']].describe())

df['average_score'] = df[['math score', 'reading score', 'writing score']].mean(axis=1)

def performance(avg):
    if avg >= 75:
        return "Good"
    elif avg >= 50:
        return "Average"
    else:
        return "At Risk"

df['performance_category'] = df['average_score'].apply(performance)

print("\nPerformance Count:")
print(df['performance_category'].value_counts())

plt.hist(df['average_score'], bins=10)
plt.title("Average Score Distribution")
plt.xlabel("Average Score")
plt.ylabel("Students")
plt.show()

df['performance_category'].value_counts().plot.pie(autopct='%1.1f%%')
plt.title("Performance Categories")
plt.ylabel("")
plt.show()

df[['math score', 'reading score', 'writing score']].mean().plot.bar()
plt.title("Subject-wise Average Scores")
plt.ylabel("Score")
plt.show()

df.groupby('gender')['average_score'].mean().plot.bar()
plt.title("Gender-wise Performance")
plt.ylabel("Average Score")
plt.show()

print("\nAnalysis Completed Successfully")
