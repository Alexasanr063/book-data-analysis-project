# book-data-analysis-project
Описание проекта
Этот проект посвящен анализу данных о продажах книг, используя набор данных, сохраненный в формате CSV. Цель проекта — загрузить данные о книгах, отфильтровать их по объему продаж, сгруппировать по жанрам и вычислить общий объем продаж для каждого жанра. В результате анализа получаем информацию о том, какие жанры продаются лучше всего.

Данные
Данные о книгах содержат следующие поля:

title: Название книги
author: Автор книги
genre: Жанр книги
sales: Объем продаж
year: Год выпуска
Пример данных:
"1984", "George Orwell", "Science Fiction", 5000, 1949
"The Lord of the Rings", "J.R.R. Tolkien", "Fantasy", 3000, 1954
"To Kill a Mockingbird", "Harper Lee", "Southern Gothic", 4000, 1960
"The Catcher in the Rye", "J.D. Salinger", "Novel", 2000, 1951
"The Great Gatsby", "F. Scott Fitzgerald", "Novel", 4500, 1925
Используемые технологии
Python
PySpark
CSV
Инструкция по запуску приложения
Для того чтобы запустить проект и выполнить анализ данных, выполните следующие шаги:

Установите необходимые библиотеки:
Убедитесь, что у вас установлен Python и библиотека PySpark. Установите её с помощью pip:
pip install pyspark
Создайте CSV файл:
Сохраните следующий код в файл с расширением .py, например book_analysis.py, чтобы создать CSV файл с данными о книгах:
import csv
data = [
    ["title", "author", "genre", "sales", "year"],
    ["1984", "George Orwell", "Science Fiction", 5000, 1949],
    ["The Lord of the Rings", "J.R.R. Tolkien", "Fantasy", 3000, 1954],
    ["To Kill a Mockingbird", "Harper Lee", "Southern Gothic", 4000, 1960],
    ["The Catcher in the Rye", "J.D. Salinger", "Novel", 2000, 1951],
    ["The Great Gatsby", "F. Scott Fitzgerald", "Novel", 4500, 1925],
]
with open("books.csv", "w", newline="", encoding="utf-8") as file:
    writer = csv.writer(file)
    writer.writerows(data)
print("CSV файл успешно создан.")
Запустите анализ данных:
Добавьте следующий код в тот же файл book_analysis.py, чтобы загрузить данные и провести анализ:
from pyspark.sql import SparkSession
from pyspark.sql.functions import sum, col
# Создаем SparkSession
spark = SparkSession.builder \
    .appName("CSV Reader") \
    .getOrCreate()
# Прочитаем CSV файл
df = spark.read.csv("books.csv", header=True, inferSchema=True)
# Покажим данные
df.show()
# Фильтруем данные
filtered_df = df.filter(df['sales'] > 3000)
# Показываем отфильтрованные данные
filtered_df.show()
# Преобразовываем столбец 'sales' в числовой тип
df = df.withColumn("sales", col("sales").cast("integer"))
# Сгруппируем данные по жанру и вычислим общий объем продаж
sales_by_genre = df.groupBy("genre").agg(sum("sales").alias("total_sales"))
# Показжим результаты
sales_by_genre.show()
# Отсортируем данные по общему объему продаж в порядке убывания
sorted_sales_by_genre = sales_by_genre.orderBy(col("total_sales").desc())
# Покажим результаты
sorted_sales_by_genre.show()
Найти еще
Запустите скрипт:
Выполните скрипт:
python book_analysis.py
Заключение
В результате анализа данных о книгах были получены сведения о жанрах и их общих объемах продаж. Мы отфильтровали книги с объемом продаж более 3000, сгруппировали данные по жанрам и отсортировали их по общему объему продаж. Это позволяет сделать выводы о предпочтениях читателей и о том, какие жанры пользуются наибольшей популярностью.
