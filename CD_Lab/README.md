# RVCE 6th Sem Compiler Design Programs

<ol>
    <li>Program 1:&nbsp;<ol>
            <li>Write a LEX program to count number of words, lines, characters and whitespaces in a given paragraph.</li>
            <li>Write a YACC program to recognize strings of the form anbn+mcm, n,m&gt;=0.</li>
        </ol>
    </li>
    <li>Program 2:<ol>
            <li>Write a LEX program to count number of Positive and Negative integers and Positive &amp; Negative fractions.</li>
            <li>Write a YACC program to validate and evaluate a simple expression involving operators +,- , * and /.</li>
        </ol>
    </li>
    <li>Program 3:<ol>
            <li>Write a LEX program to count the number of comment lines in a C Program. Also eliminate them and copy that program into a separate file.</li>
            <li>Write a YACC program to recognize a nested (minimum3levels)FOR loop statement for C language.</li>
        </ol>
    </li>
    <li>Program 4:<ol>
            <li>Write a LEX program to recognize and count the number of identifiers, operators and keywords in a given input file.</li>
            <li>Write a YACC program to recognize nested IF control statements (C language) and display the number of levels of nesting.</li>
        </ol>
    </li>
    <li>Program 5: Write a YACC program to recognize Declaration statement (C language) and 
display the number variables declared . 
Variable can be any basic data type  or array type 
Example int a[10],a,b,c;   </li>
    <li>Program 6: YACC program that reads the C statements for an input file and converts them in quadruple three address intermediate code.</li>
    <li>Program 7: Write a YACC program that identifies the Function Definition of C language.</li>
    <li>Program 8: Write a YACC program that generates Assembly language (Target) Code for valid Arithmetic Expression.</li>
</ol>
<ol>
    <p>Commands for execution:</p>
    <li>For lex programs:</li>
    <p>lex file.l</p>
    <p>gcc lex.yy.c</p>
    <p>./a.out</p>
    <li>For yacc programs:</li>
    <p>lex file.l</p>
    <p>yacc -d file.y</p>
    <p>gcc lex.yy.c yy.tab.c</p>
    <p>./a.out</p>
</ol>
