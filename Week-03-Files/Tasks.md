<!DOCTYPE html>
<html lang="bg">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>C++ Задачи</title>
<style>
  body { font-family: 'Segoe UI', Arial, sans-serif; max-width: 860px; margin: 40px auto; padding: 0 24px; background: #f8f9fa; color: #212529; }
  h1 { color: #1a1a2e; border-bottom: 3px solid #4361ee; padding-bottom: 10px; }
  h2 { color: #1a1a2e; margin-top: 40px; }
  .task-block { background: white; border-radius: 10px; padding: 24px; margin-bottom: 28px; box-shadow: 0 2px 8px rgba(0,0,0,0.08); }
  .task-block ul { margin: 8px 0 8px 20px; }
  .task-block li { margin-bottom: 4px; }
  details { margin-top: 18px; }
  summary {
    cursor: pointer;
    background: #4361ee;
    color: white;
    padding: 10px 18px;
    border-radius: 6px;
    font-weight: bold;
    font-size: 15px;
    user-select: none;
    list-style: none;
    display: flex;
    align-items: center;
    gap: 8px;
  }
  summary::-webkit-details-marker { display: none; }
  summary::before { content: "▶"; font-size: 11px; transition: transform 0.2s; }
  details[open] summary::before { transform: rotate(90deg); }
  summary:hover { background: #3451d1; }
  pre {
    background: #1e1e2e;
    color: #cdd6f4;
    padding: 20px;
    border-radius: 8px;
    overflow-x: auto;
    font-size: 13.5px;
    line-height: 1.6;
    margin-top: 14px;
    font-family: 'Consolas', 'Fira Code', monospace;
  }
  code { font-family: 'Consolas', 'Fira Code', monospace; background: #e9ecef; padding: 2px 5px; border-radius: 3px; font-size: 13px; }
  .note { background: #fff3cd; border-left: 4px solid #ffc107; padding: 10px 14px; border-radius: 4px; margin-top: 10px; font-size: 14px; }
  .keyword { color: #cba6f7; }
  .type    { color: #89b4fa; }
  .string  { color: #a6e3a1; }
  .comment { color: #6c7086; font-style: italic; }
  .func    { color: #89dceb; }
</style>
</head>
<body>

<h1>C++ Задачи — Файлове и Структури</h1>

<!-- ЗАДАЧА 1 -->
<div class="task-block">
  <h2>Задача 1</h2>
  <p>Напишете програма, която отпечатва собствения си код.</p>

  <details>
    <summary>Решение</summary>
<pre><span class="comment">// Task1_Quine.cpp</span>
<span class="keyword">#include</span> <span class="string">&lt;iostream&gt;</span>
<span class="keyword">#include</span> <span class="string">&lt;fstream&gt;</span>

<span class="type">int</span> <span class="func">main</span>() {
    <span class="comment">// Отваряме самия изходен файл за четене</span>
    std::ifstream file(<span class="string">"Task1_Quine.cpp"</span>);

    <span class="keyword">if</span> (!file.is_open()) {
        std::cout &lt;&lt; <span class="string">"Грешка: файлът не може да бъде отворен!"</span> &lt;&lt; std::endl;
        <span class="keyword">return</span> 1;
    }

    <span class="type">char</span> ch;
    <span class="keyword">while</span> (file.get(ch)) {
        std::cout &lt;&lt; ch;
    }

    file.close();
    <span class="keyword">return</span> 0;
}</pre>
    <div class="note">💡 Програмата отваря собствения си <code>.cpp</code> файл и го отпечатва символ по символ. Файлът трябва да се казва точно <code>Task1_Quine.cpp</code>.</div>
  </details>
</div>

<!-- ЗАДАЧА 2 -->
<div class="task-block">
  <h2>Задача 2</h2>
  <p>Напишете функция, която приема име на съществуващ файл и копира съдържанието му в нов файл.</p>

  <details>
    <summary>Решение</summary>
<pre><span class="keyword">#include</span> <span class="string">&lt;iostream&gt;</span>
<span class="keyword">#include</span> <span class="string">&lt;fstream&gt;</span>

<span class="type">void</span> <span class="func">copyFile</span>(<span class="keyword">const</span> <span class="type">char</span>* sourceName, <span class="keyword">const</span> <span class="type">char</span>* destName) {
    std::ifstream source(sourceName, std::ios::binary);
    <span class="keyword">if</span> (!source.is_open()) {
        std::cout &lt;&lt; <span class="string">"Грешка: не може да се отвори "</span> &lt;&lt; sourceName &lt;&lt; std::endl;
        <span class="keyword">return</span>;
    }

    std::ofstream dest(destName, std::ios::binary);
    <span class="keyword">if</span> (!dest.is_open()) {
        std::cout &lt;&lt; <span class="string">"Грешка: не може да се създаде "</span> &lt;&lt; destName &lt;&lt; std::endl;
        source.close();
        <span class="keyword">return</span>;
    }

    <span class="comment">// Четем и записваме блокове от 512 байта</span>
    <span class="type">char</span> buffer[512];
    <span class="keyword">while</span> (source.read(buffer, <span class="keyword">sizeof</span>(buffer))) {
        dest.write(buffer, source.gcount());
    }
    <span class="comment">// Записваме последния непълен блок (ако има)</span>
    dest.write(buffer, source.gcount());

    source.close();
    dest.close();
    std::cout &lt;&lt; <span class="string">"Файлът е копиран успешно!"</span> &lt;&lt; std::endl;
}

<span class="type">int</span> <span class="func">main</span>() {
    <span class="func">copyFile</span>(<span class="string">"source.txt"</span>, <span class="string">"copy.txt"</span>);
    <span class="keyword">return</span> 0;
}</pre>
    <div class="note">💡 Използваме <code>std::ios::binary</code>, за да работи правилно и с нетекстови файлове. Четем на парчета (buffer), за да не зареждаме целия файл в паметта наведнъж.</div>
  </details>
</div>

<!-- ЗАДАЧА 3 -->
<div class="task-block">
  <h2>Задача 3</h2>
  <p>Имплементирайте структура <code>Student</code> с факултетен номер, име и среден успех. Реализирайте методи <code>write/read</code> и функции <code>writeStudents/readStudents</code>.</p>

  <details>
    <summary>Решение</summary>
<pre><span class="keyword">#include</span> <span class="string">&lt;iostream&gt;</span>
<span class="keyword">#include</span> <span class="string">&lt;fstream&gt;</span>
<span class="keyword">#include</span> <span class="string">&lt;cstring&gt;</span>

<span class="keyword">struct</span> <span class="type">Student</span> {
    <span class="type">int</span>    fn;          <span class="comment">// факултетен номер</span>
    <span class="type">char</span>   name[64];   <span class="comment">// име</span>
    <span class="type">double</span> grade;      <span class="comment">// среден успех</span>

    <span class="comment">// Записва студента в поток (бинарно)</span>
    <span class="type">void</span> <span class="func">write</span>(std::ofstream&amp; out) <span class="keyword">const</span> {
        out.write((<span class="keyword">const</span> <span class="type">char</span>*)&amp;fn,    <span class="keyword">sizeof</span>(fn));
        out.write((<span class="keyword">const</span> <span class="type">char</span>*)name,  <span class="keyword">sizeof</span>(name));
        out.write((<span class="keyword">const</span> <span class="type">char</span>*)&amp;grade, <span class="keyword">sizeof</span>(grade));
    }

    <span class="comment">// Чете студента от поток (бинарно)</span>
    <span class="type">void</span> <span class="func">read</span>(std::ifstream&amp; in) {
        in.read((<span class="type">char</span>*)&amp;fn,    <span class="keyword">sizeof</span>(fn));
        in.read((<span class="type">char</span>*)name,  <span class="keyword">sizeof</span>(name));
        in.read((<span class="type">char</span>*)&amp;grade, <span class="keyword">sizeof</span>(grade));
    }

    <span class="type">void</span> <span class="func">print</span>() <span class="keyword">const</span> {
        std::cout &lt;&lt; <span class="string">"FN: "</span> &lt;&lt; fn
                  &lt;&lt; <span class="string">", Name: "</span> &lt;&lt; name
                  &lt;&lt; <span class="string">", Grade: "</span> &lt;&lt; grade &lt;&lt; std::endl;
    }
};

<span class="comment">// Записва масив от студенти във файл</span>
<span class="type">void</span> <span class="func">writeStudents</span>(<span class="keyword">const</span> <span class="type">Student</span>* students, <span class="type">int</span> count, std::ofstream&amp; out) {
    out.write((<span class="keyword">const</span> <span class="type">char</span>*)&amp;count, <span class="keyword">sizeof</span>(count)); <span class="comment">// записваме броя</span>
    <span class="keyword">for</span> (<span class="type">int</span> i = 0; i &lt; count; i++) {
        students[i].write(out);
    }
}

<span class="comment">// Чете масив от студенти от файл; връща броя на прочетените</span>
<span class="type">int</span> <span class="func">readStudents</span>(<span class="type">Student</span>* students, <span class="type">int</span> maxCount, std::ifstream&amp; in) {
    <span class="type">int</span> count = 0;
    in.read((<span class="type">char</span>*)&amp;count, <span class="keyword">sizeof</span>(count));
    <span class="keyword">if</span> (count &gt; maxCount) count = maxCount;
    <span class="keyword">for</span> (<span class="type">int</span> i = 0; i &lt; count; i++) {
        students[i].read(in);
    }
    <span class="keyword">return</span> count;
}

<span class="type">int</span> <span class="func">main</span>() {
    <span class="comment">// --- Запис ---</span>
    <span class="type">Student</span> arr[3];
    arr[0] = { 70001, <span class="string">"Ivan Ivanov"</span>, 5.50 };
    arr[1] = { 70002, <span class="string">"Maria Petrova"</span>, 4.75 };
    arr[2] = { 70003, <span class="string">"Georgi Georgiev"</span>, 6.00 };

    std::ofstream out(<span class="string">"students.bin"</span>, std::ios::binary);
    <span class="func">writeStudents</span>(arr, 3, out);
    out.close();

    <span class="comment">// --- Четене ---</span>
    <span class="type">Student</span> loaded[10];
    std::ifstream in(<span class="string">"students.bin"</span>, std::ios::binary);
    <span class="type">int</span> n = <span class="func">readStudents</span>(loaded, 10, in);
    in.close();

    std::cout &lt;&lt; <span class="string">"Прочетени студенти: "</span> &lt;&lt; n &lt;&lt; std::endl;
    <span class="keyword">for</span> (<span class="type">int</span> i = 0; i &lt; n; i++) {
        loaded[i].print();
    }
    <span class="keyword">return</span> 0;
}</pre>
    <div class="note">💡 Данните се записват в <b>бинарен</b> формат — по-бързо и компактно. Броят на студентите се записва в началото на файла, за да знаем колко да четем при зареждане.</div>
  </details>
</div>

<!-- ЗАДАЧА 4 -->
<div class="task-block">
  <h2>Задача 4</h2>
  <p>Програма, която чете CSV файл със студенти (Първо име, Фамилно име, Имейл, Факултетен номер) и предоставя конзолен интерфейс за търсене, редакция и запазване.</p>
  <p><strong>Пример:</strong></p>
  <pre style="background:#f1f3f5; color:#212529; padding:14px; border-radius:6px; font-size:13px;">Open file:
&gt;students.csv
File successfully opened!
&gt;print 70000
Name = Ivan Ivanov, Email: ivan@gmail.com, FN: 70000
&gt;edit 70000 ivan@abv.bg
&gt;print 70000
Name = Ivan Ivanov, Email: ivan@abv.bg, FN: 70000
&gt;save students2.csv
file students2.csv successfully saved!</pre>

  <details>
    <summary>Решение</summary>
<pre><span class="keyword">#include</span> <span class="string">&lt;iostream&gt;</span>
<span class="keyword">#include</span> <span class="string">&lt;fstream&gt;</span>
<span class="keyword">#include</span> <span class="string">&lt;cstring&gt;</span>
<span class="keyword">#include</span> <span class="string">&lt;cstdlib&gt;</span>

<span class="keyword">const</span> <span class="type">int</span> MAX_STUDENTS = 200;
<span class="keyword">const</span> <span class="type">int</span> MAX_STR     = 100;

<span class="keyword">struct</span> <span class="type">Student</span> {
    <span class="type">char</span> firstName[MAX_STR];
    <span class="type">char</span> lastName[MAX_STR];
    <span class="type">char</span> email[MAX_STR];
    <span class="type">int</span>  fn;
};

<span class="comment">// ─── Помощна функция: разделя ред по запетая ───────────────────────</span>
<span class="type">void</span> <span class="func">parseLine</span>(<span class="type">char</span>* line, <span class="type">Student</span>&amp; s) {
    <span class="type">char</span>* token = strtok(line, <span class="string">","</span>);
    <span class="keyword">if</span> (token) strncpy(s.firstName, token, MAX_STR - 1);

    token = strtok(<span class="keyword">nullptr</span>, <span class="string">","</span>);
    <span class="keyword">if</span> (token) strncpy(s.lastName, token, MAX_STR - 1);

    token = strtok(<span class="keyword">nullptr</span>, <span class="string">","</span>);
    <span class="keyword">if</span> (token) strncpy(s.email, token, MAX_STR - 1);

    token = strtok(<span class="keyword">nullptr</span>, <span class="string">","</span>);
    <span class="keyword">if</span> (token) s.fn = atoi(token);
}

<span class="comment">// ─── Зарежда CSV файл ──────────────────────────────────────────────</span>
<span class="type">int</span> <span class="func">loadFile</span>(<span class="keyword">const</span> <span class="type">char</span>* filename, <span class="type">Student</span> students[]) {
    std::ifstream file(filename);
    <span class="keyword">if</span> (!file.is_open()) <span class="keyword">return</span> -1;

    <span class="type">int</span> count = 0;
    <span class="type">char</span> line[512];
    <span class="keyword">while</span> (file.getline(line, <span class="keyword">sizeof</span>(line)) &amp;&amp; count &lt; MAX_STUDENTS) {
        <span class="keyword">if</span> (strlen(line) &gt; 2) {          <span class="comment">// пропускаме празни редове</span>
            <span class="func">parseLine</span>(line, students[count]);
            count++;
        }
    }
    file.close();
    <span class="keyword">return</span> count;
}

<span class="comment">// ─── Отпечатва студент по факултетен номер ─────────────────────────</span>
<span class="type">void</span> <span class="func">printStudent</span>(<span class="keyword">const</span> <span class="type">Student</span> students[], <span class="type">int</span> count, <span class="type">int</span> fn) {
    <span class="keyword">for</span> (<span class="type">int</span> i = 0; i &lt; count; i++) {
        <span class="keyword">if</span> (students[i].fn == fn) {
            std::cout &lt;&lt; <span class="string">"Name = "</span> &lt;&lt; students[i].firstName
                      &lt;&lt; <span class="string">" "</span>    &lt;&lt; students[i].lastName
                      &lt;&lt; <span class="string">", Email: "</span> &lt;&lt; students[i].email
                      &lt;&lt; <span class="string">", FN: "</span>    &lt;&lt; students[i].fn
                      &lt;&lt; std::endl;
            <span class="keyword">return</span>;
        }
    }
    std::cout &lt;&lt; <span class="string">"Студент с FN "</span> &lt;&lt; fn &lt;&lt; <span class="string">" не е намерен."</span> &lt;&lt; std::endl;
}

<span class="comment">// ─── Променя email по факултетен номер ────────────────────────────</span>
<span class="type">void</span> <span class="func">editEmail</span>(<span class="type">Student</span> students[], <span class="type">int</span> count, <span class="type">int</span> fn, <span class="keyword">const</span> <span class="type">char</span>* newEmail) {
    <span class="keyword">for</span> (<span class="type">int</span> i = 0; i &lt; count; i++) {
        <span class="keyword">if</span> (students[i].fn == fn) {
            strncpy(students[i].email, newEmail, MAX_STR - 1);
            <span class="keyword">return</span>;
        }
    }
    std::cout &lt;&lt; <span class="string">"Студент с FN "</span> &lt;&lt; fn &lt;&lt; <span class="string">" не е намерен."</span> &lt;&lt; std::endl;
}

<span class="comment">// ─── Запазва студентите в CSV файл ────────────────────────────────</span>
<span class="type">void</span> <span class="func">saveFile</span>(<span class="keyword">const</span> <span class="type">char</span>* filename, <span class="keyword">const</span> <span class="type">Student</span> students[], <span class="type">int</span> count) {
    std::ofstream file(filename);
    <span class="keyword">if</span> (!file.is_open()) {
        std::cout &lt;&lt; <span class="string">"Грешка при записване на файла!"</span> &lt;&lt; std::endl;
        <span class="keyword">return</span>;
    }
    <span class="keyword">for</span> (<span class="type">int</span> i = 0; i &lt; count; i++) {
        file &lt;&lt; students[i].firstName &lt;&lt; <span class="string">","</span>
             &lt;&lt; students[i].lastName  &lt;&lt; <span class="string">","</span>
             &lt;&lt; students[i].email     &lt;&lt; <span class="string">","</span>
             &lt;&lt; students[i].fn        &lt;&lt; <span class="string">"\n"</span>;
    }
    file.close();
    std::cout &lt;&lt; <span class="string">"file "</span> &lt;&lt; filename &lt;&lt; <span class="string">" successfully saved!"</span> &lt;&lt; std::endl;
}

<span class="comment">// ─── Main: конзолен интерфейс ─────────────────────────────────────</span>
<span class="type">int</span> <span class="func">main</span>() {
    <span class="type">Student</span> students[MAX_STUDENTS];
    <span class="type">int</span> count = 0;

    <span class="comment">// Зареждане на файл</span>
    std::cout &lt;&lt; <span class="string">"Open file: \n&gt;"</span>;
    <span class="type">char</span> filename[MAX_STR];
    std::cin.getline(filename, MAX_STR);

    count = <span class="func">loadFile</span>(filename, students);
    <span class="keyword">if</span> (count &lt; 0) {
        std::cout &lt;&lt; <span class="string">"Грешка: файлът не може да бъде отворен!"</span> &lt;&lt; std::endl;
        <span class="keyword">return</span> 1;
    }
    std::cout &lt;&lt; <span class="string">"File successfully opened!"</span> &lt;&lt; std::endl;

    <span class="comment">// Команден цикъл</span>
    <span class="type">char</span> line[512];
    <span class="keyword">while</span> (<span class="keyword">true</span>) {
        std::cout &lt;&lt; <span class="string">"&gt;"</span>;
        <span class="keyword">if</span> (!std::cin.getline(line, <span class="keyword">sizeof</span>(line))) <span class="keyword">break</span>;

        <span class="type">char</span> cmd[32] = {};
        sscanf(line, <span class="string">"%31s"</span>, cmd);

        <span class="keyword">if</span> (strcmp(cmd, <span class="string">"print"</span>) == 0) {
            <span class="type">int</span> fn;
            sscanf(line, <span class="string">"%*s %d"</span>, &amp;fn);
            <span class="func">printStudent</span>(students, count, fn);

        } <span class="keyword">else if</span> (strcmp(cmd, <span class="string">"edit"</span>) == 0) {
            <span class="type">int</span>  fn;
            <span class="type">char</span> newEmail[MAX_STR] = {};
            sscanf(line, <span class="string">"%*s %d %99s"</span>, &amp;fn, newEmail);
            <span class="func">editEmail</span>(students, count, fn, newEmail);

        } <span class="keyword">else if</span> (strcmp(cmd, <span class="string">"save"</span>) == 0) {
            <span class="type">char</span> outFile[MAX_STR] = {};
            sscanf(line, <span class="string">"%*s %99s"</span>, outFile);
            <span class="func">saveFile</span>(outFile, students, count);

        } <span class="keyword">else if</span> (strcmp(cmd, <span class="string">"exit"</span>) == 0) {
            <span class="keyword">break</span>;
        } <span class="keyword">else</span> {
            std::cout &lt;&lt; <span class="string">"Непозната команда. Използвайте: print &lt;fn&gt; | edit &lt;fn&gt; &lt;email&gt; | save &lt;file&gt; | exit"</span> &lt;&lt; std::endl;
        }
    }
    <span class="keyword">return</span> 0;
}</pre>
    <div class="note">💡 Примерно <code>students.csv</code>:<br>
    <code>Ivan,Ivanov,ivan@gmail.com,70000</code><br>
    <code>Maria,Petrova,maria@abv.bg,70001</code><br><br>
    Поддържани команди: <b>print &lt;fn&gt;</b>, <b>edit &lt;fn&gt; &lt;email&gt;</b>, <b>save &lt;filename&gt;</b>, <b>exit</b>.</div>
  </details>
</div>

</body>
</html>