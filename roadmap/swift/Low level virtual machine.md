Делится на три части: 
- `Frontend` - задачей front end’а является преобразование языка высокого уровня (например, Swift’а) в IR.
- `Middle` - ответственен за оптимизацию этого промежуточного представления.
- `Backend` - преобразует это представление в машинный код под конкретную архитектуру.

# Front end
На вход компилятору поступает код, написанный на языке высокого уровня. В данном случае это будет Swift.

Первый этап называется Parsing. Парсер ответственен за генерацию абстрактного синтаксического дерева — AST (Abstract Syntax Tree) без какой-либо семантической информации о типах.
- Затем наступает очередь семантического анализа (Semantic analysis). В результате чего формируется уже семантически корректное типизированное синтаксическое дерево. На этом этапе определяется наличие семантических ошибок.
- На следующем шаге происходит импорт кода, написанного на Objective-C и C, в Swift’овое представление. За это отвечает ClangImporter.
- И на последнем этапе происходит генерация Swift Intermediate Language в его первой форме представления.

![image](https://user-images.githubusercontent.com/47610132/180501381-d3e99733-0041-4ec2-9ca9-de002d2503c7.png)

# Middle
- Первая форма SIL поступает на вход оптимизатору. Который выполняет анализ, оптимизацию и трансформацию этой формы в каноническую.
- Сначала на шаге трансформации выполняется диагностика потока данных и гарантированные преобразования, повышающие производительность программы (определяется использование неинициализированных переменных, проверяется достижимость всех узлов графа потока данных).
- Дальше SILOptimizer выполняет высокоуровневые доменно-специфичные оптимизации для базовых типов контейнеров. Также он ответственен за девиртуализацию функций, оптимизацию ARC и спецификацию дженериков.
- После этого уже генерируется промежуточное представление **LLVM**, которое поступает на вход **Back-end’у**.

![image](https://user-images.githubusercontent.com/47610132/180501752-b794683e-8fb2-40b1-9ba2-7d7ca83f9e69.png)

# Backend
- Почему-то многие, когда говорят о back-end’е LLVM, имеют ввиду сам LLVM. Хотя на деле этим back-end’ом является LLC (LLVM Static Compiler).
- LLC выполняет уже специфичные для конкретных архитектур оптимизации — использование меньшего места на диске, повышение скорости работы, понижение энергозатрат. 
Также он генерирует машинный код под конкретную архитектуру (x86, ARM, MIPS, и т.д.). Собирает объектные файлы в конечное приложение, библиотеку или фрэймворк.

![image](https://user-images.githubusercontent.com/47610132/180502050-7a204694-57d6-443e-ba1b-0e2156ae90cf.png)

![image](https://user-images.githubusercontent.com/47610132/180502240-8461bd4b-3f88-4930-b89b-49808fa153d9.png)

# Подробные статьи
- [Статья](https://habr.com/ru/company/e-legion/blog/438204/) номер один
- [Статья](https://habr.com/ru/company/e-legion/blog/438664/) номер два
- [Статья](https://habr.com/ru/company/e-legion/blog/438696/) номер три
- [Статья](https://habr.com/ru/company/e-legion/blog/440078/) номер четыре