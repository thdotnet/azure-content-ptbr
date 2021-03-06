<properties 
	pageTitle="Material de referência da Análise no Application Insights" 
	description="Expressões regulares na Análise, a ferramenta de pesquisa avançada do Application Insights." 
	services="application-insights" 
    documentationCenter=""
	authors="alancameronwills" 
	manager="douge"/>

<tags 
	ms.service="application-insights" 
	ms.workload="tbd" 
	ms.tgt_pltfrm="ibiza" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="03/21/2016" 
	ms.author="awills"/>

# Application Insights: Material de referência da Análise

A [Análise](app-analytics.md) permite executar consultas avançadas na telemetria de seu aplicativo coletada pelo [Application Insights](app-insights-overview.md). Essas páginas descrevem a linguagem de consulta.


[AZURE.INCLUDE [app-analytics-top-index](../../includes/app-analytics-top-index.md)]

## Expressões regulares


[> Descrição geral das expressões regulares](https://github.com/google/re2/wiki/Syntax).

Esta página lista a sintaxe de expressão regular aceita pelo RE2. 
Ela também lista a sintaxe aceita pelo PCRE, PERL e VIM.

||
|---|---
|Caracteres únicos: | 
|. |qualquer caractere, possivelmente incluindo o de nova linha (s=true) 
|[xyz] |classe de caractere 
|[^xyz] |classe de caractere negada 
|\\d |classe de caractere Perl 
|\\D |classe de caractere Perl negada 
|[[:alpha:]] |classe de caractere ASCII 
|[[:^alpha:]] |classe de caractere ASCII negada 
|\\pN |classe de caractere Unicode (nome de uma letra) 
|\\p{Greek} |classe de caractere Unicode 
|\\PN |classe de caractere Unicode negada (nome de uma letra) 
|\\P{Greek} |classe de caractere Unicode negada 
|Composições: | 
|xy |x seguido de y 
|x&#124;y |x ou y (preferir x) 
| 
|Repetições: | 
|zero ou mais x, prefira mais 
|x+ |um ou mais x, prefira mais 
|x? |zero ou um x, prefira um 
|x{n,m} |n ou n+1 ou... ou m x, prefira mais 
|x{n,} |n ou mais x, prefira mais 
|x{n} |exatamente n x 
|x*? |zero ou mais x, prefira menos 
|x+? |um ou mais x, prefira menos 
|x?? |zero ou um x, prefira zero 
|x{n,m}? | n ou n+1 ou... ou m x, prefira menos 
|x{n}? |n ou mais x, prefira menos 
|x{n}? |exatamente x n 
|x{} |(== x*) (SEM SUPORTE) VIM 
|x{-} |(== x*?) (SEM SUPORTE) VIM 
|x{-n} |(== x{n}?) (SEM SUPORTE) VIM 
|x= |(== x?) (SEM SUPORTE) VIM 
| Restrição de implementação: os formulários de contagem x{n,m}, x{n,}, e x{n} | 
|rejeitam formulários que criam uma contagem de repetição mínima ou máxima acima de 1000. | 
| Repetições ilimitadas não estão sujeitas a essa restrição. | 
| Repetições possessivas: | 
|x*+ |zero ou mais x, possessiva (SEM SUPORTE) 
|x++ |um ou mais x, possessiva (SEM SUPORTE) 
|x?+ |zero ou um x, possessiva (SEM SUPORTE) 
|x{n, m}+ |n ou... ou m x, possessiva (SEM SUPORTE) 
|x{n}+ |n ou mais x, possessiva (SEM SUPORTE) 
|x{n}+ |exatamente n x, possessiva (SEM SUPORTE) 
| Agrupamento: | 
|(re)| grupo de captura numerado (subcorrespondência) 
|(?P<name>re) |grupo de captura nomeado e numerado (subcorrespondência) 
| (?<name>re) |grupo de captura nomeado e numerado (subcorrespondência) (SEM SUPORTE) 
| (?'name're) | grupo de captura nomeado e numerado (subcorrespondência) (SEM SUPORTE) 
|(?:re)| grupo de não captura 
|(? sinalizadores) |definir sinalizadores dentro do grupo atual; não captura 
|(?flags:re) |definir sinalizadores durante re; não captura 
|(?#text) |comentário (SEM SUPORTE) 
|(?&#124;x&#124;y&#124;z) |redefinição de numeração de ramificação (SEM SUPORTE) 
|(?>re) |correspondência possessiva de re (SEM SUPORTE) 
|re@> |correspondência possessiva de re (SEM SUPORTE) VIM 
|%(re) |grupo de não captura (SEM SUPORTE) VIM 
|Sinalizadores: | 
|i |não diferencia maiúsculas de minúsculas (padrão false) 
|m |modo multilinha: ^ e $ correspondem ao início/término da linha além do início/término do texto (padrão false) 
|s |let . match \\n (padrão false) 
|U |ungreedy: trocar significado de x* e x*?, x+ e x+?, etc (padrão false) 
|Sintaxe do sinalizador é xyz (set) ou -xyz (clear) ou xy-z (set xy, clear z). | 
|Cadeias de caractere vazias: | 
|^ |no início do texto ou linha (m=true) 
|$ |ao final do texto (como \\z não \\Z) ou linha (m=true) 
|\\A |no início do texto 
|\\b |no limite da palavra ASCII (\\w em um lado e \\W, \\A ou \\z no outro)
|\\B |não é um limite de palavra ASCII 
|\\G |no início do subtexto pesquisado (SEM SUPORTE) PCRE 
|\\G |ao final da última correspondência (SEM SUPORTE) PERL 
|\\Z |ao final do texto, ou antes de nova linha ao final do texto (SEM SUPORTE) 
|\\z |ao final do texto 
|(?=re) |antes do texto correspondente a re (SEM SUPORTE) 
|(?!re) |antes do texto correspondente a re (SEM SUPORTE) 
|(?<=re) |após o texto correspondente a re (SEM SUPORTE) 
|(?<!re) |após o texto não correspondente a re (SEM SUPORTE) 
|re& |antes do texto correspondente a re (SEM SUPORTE) VIM 
|re@= |antes do texto correspondente a re (SEM SUPORTE) VIM 
|re@! |antes do texto não correspondente a re (SEM SUPORTE) VIM 
|re@<= |após o texto correspondente a re (SEM SUPORTE) VIM 
|re@<! |após o texto não correspondente a re (SEM SUPORTE) VIM
|\\zs |define o início da correspondência (= \\K) (SEM SUPORTE) VIM 
|\\ze | define o término da correspondência (SEM SUPORTE) VIM 
|\\%^ |início do arquivo (SEM SUPORTE) VIM 
|\\%$ |final do arquivo (SEM SUPORTE) VIM 
|\\%V |na tela (SEM SUPORTE) VIM 
|\\%# |posição do cursor (SEM SUPORTE) VIM 
|\\%'m |marcar posição m (SEM SUPORTE) VIM 
|\\%23l |na linha 23 (SEM SUPORTE) VIM 
|\\%23c |na coluna 23 (SEM SUPORTE) VIM 
|\\%23v |na coluna virtual 23 (SEM SUPORTE) VIM 
|Sequências de escape: | 
|\\a |bell (== \\007) 
|\\f | feed de formulário (== \\014) 
|\\t |tabulação horizontal (== \\011) 
|\\n |nova linha (== \\012) 
|\\r |retorno de carro (== \\015) 
|\\v |caractere de tabulação vertical (== \\013) 
|* |literal *, para qualquer caractere de pontuação * 
|\\123 |código do caractere octal (até três dígitos) 
|\\x7F |código do caractere hexadecimal (exatamente dois dígitos) 
|\\x{10FFFF} |código de caractere hexadecimal 
|\\C |correspondente um byte único mesmo no modo UTF-8 
|\\Q...\\E | texto literal ... mesmo se... tiver pontuação 
|\\1 |referência inversa (SEM SUPORTE) 
|\\b |Backspace (SEM SUPORTE) (use \\010) 
|\\cK | caractere de controle ^K (SEM SUPORTE) (use \\001 etc) 
|\\e |escape (SEM SUPORTE) (use \\033) 
|\\g1 |referência inversa (SEM SUPORTE) 
|\\g{1} |referência inversa (SEM SUPORTE) 
|\\g{+1} |referência inversa (SEM SUPORTE) 
|\\g{-1} |referência inversa (SEM SUPORTE) 
|\\g{name} | referência inversa nomeada (SEM SUPORTE) 
|\\g<name> |chamada de sub-rotina (SEM SUPORTE) 
|\\g'name' |chamada de sub-rotina (SEM SUPORTE) 
|\\k<name> | referência inversa nomeada (SEM SUPORTE) 
|\\k'name' | referência inversa nomeada (SEM SUPORTE) 
|\\lX |X minúsculo (SEM SUPORTE) 
|\\ux | x maiúsculo (SEM SUPORTE) 
|\\L...\\E |texto minúsculo... (SEM SUPORTE) 
|\\K | início da redefinição de $0 (SEM SUPORTE) 
|\\N{name} | caractere Unicode nomeado (SEM SUPORTE) 
|\\R | quebra de linha (SEM SUPORTE) 
|\\U … \\E |texto em maiúscula... (SEM SUPORTE) 
|\\X |Sequência Unicode estendida (SEM SUPORTE) 
|\\%d123 |caractere decimal 123 (SEM SUPORTE) VIM 
|\\%xFF |caractere hexadecimal FF (SEM SUPORTE) VIM 
|\\%o123 |caractere octal 123 (SEM SUPORTE) VIM 
|\\%u1234 |caractere Unicode 0x1234 (SEM SUPORTE) VIM 
|\\%U12345678 |caractere Unicode 0x12345678 (SEM SUPORTE) VIM 
|Elementos de classe de caractere: | 
|x |caractere único 
|A-Z |intervalo de caracteres (inclusive) 
|\\d |classe de caractere Perl 
|[:foo:] |foo de classe de caractere ASCII 
|\\p{Foo} |foo de classe de caractere Unicode 
|\\pF |Classe de caractere Unicode F (nome de uma letra) 
|Classes de caractere nomeado como elementos de classe de caractere: | 
|[\\d] |dígitos (== \\d) 
|[^\\d] |não dígitos (== \\D) 
|[\\D] |não dígitos (== \\D) 
|[^\\D] | não dígitos (== \\d) 
|[[:name:]] |classe ASCII nomeada dentro da classe de caractere (== [:name:]) 
|[^[:name:]] |classe ASCII nomeada dentro da classe de caracteres negada (== [:^name:]) 
|[\\p{Name}] |propriedade Unicode nomeada dentro da classe de caractere (== \\p{Name}) 
|[^\\p{Name}] |propriedade Unicode nomeada dentro da classe de caractere negada (== \\P{Name}) 
|classes de caractere Perl (todas somente ASCII): | 
|\\d |dígitos (== [0-9]) 
|\\D |não dígitos (== [^0-9]) 
|\\s |espaço em branco (== [\\t\\n\\f\\r ]) 
|\\S |não espaço em branco (== [^\\t\\n\\f\\r ]) 
|\\w |caracteres de palavra (== [0-9A-Za-z\_]) 
|\\W |não caracteres de palavra (== [^0-9A-Za-z\_]) 
|\\h |espaço horizontal (SEM SUPORTE) 
|\\H |não espaço horizontal (SEM SUPORTE) 
|\\v |espaço vertical (SEM SUPORTE) 
|\\V |não espaço vertical (SEM SUPORTE) 
|classes de caractere ASCII: | 
|[[:alnum:]] |alfanumérico (== [0-9A-Za-z]) 
|[[:alpha:]] |alfabético (== [A-Za-z]) 
|[[:ascii:]] |ASCII (== [\\x00-\\x7F]) 
|[[:blank:]] |em branco (== [\\t ]) 
|[[:cntrl:]] |control (== [\\x00-\\x1F\\x7F]) 
|[[:digit:]] |dígitos (== [0-9]) 
|[[:graph:]] |elemento gráfico (== [!-~] == [A-Za-z0-9!"#$%&'()*+,-./:;<=>?@[\\]^\_`{&#124;}~]) 
|[[:lower:]] |lower case (== [a-z]) 
|[[:print:]] |printable (== [ -~] == [ [:graph:]]) 
|[[:punct:]] |punctuation (== [!-/:-@[-`{-~]) 
|[[:space:]] |espaço em branco (== [\\t\\n\\v\\f\\r ]) 
|[[:upper:]] |maiúscula (== [A-Z]) 
|[[:word:]] |caracteres de palavra (== [0-9A-Za-z\_]) 
|[[:xdigit:]] |dígito hexadecimal (== [0-9A-Fa-f]) 
|nomes de classe de caractere Unicode –categoria geral: | 
|C |outro 
|Cc |controle 
|Cf |formato 
|Cn |pontos de código não atribuídos (SEM SUPORTE) 
|Co |uso particular 
|Cs |surrogate 
|L |letra 
|LC |letra capitalizada (SEM SUPORTE) 
|L& | letra capitalizada (SEM SUPORTE) 
|Ll |letra minúscula 
|Lm |letra modificada 
|Lo |outra letra 
|Lt |letra inicial maiúscula 
|Lu |letra maiúscula 
|M |marca 
|Mc |marca de espaçamento 
|Me |marca delimitadora 
|Mn |marca de não espaçamento 
|N |número 
|Nd |número decimal 
|Nl |número de letra 
|No |outro número 
|P |pontuação 
|Pc |pontuação conectora 
|Pd |pontuação de traço 
|Pe |pontuação de fechamento 
|Pf |pontuação final 
|Pi |pontuação inicial 
|Po |outra pontuação 
|Ps |pontuação de abertura 
|S |símbolo 
|Sc |símbolo monetário 
|Sk |símbolo modificador 
|Sm |símbolo matemático 
|So |outro símbolo 
|Z |separador 
|Zl |separador de linha 
|Zp |separador de parágrafo 
|Zs |separador de espaço 
|nomes de classe de caractere Unicode—scripts: | 
| Árabe | Árabe 
| Armênio | Armênio 
| Balinês | Balinês 
| Bamum | Bamum 
| Batak | Batak 
| Bengali | Bengali 
| Bopomofo | Bopomofo 
| Brahmi | Brahmi 
| Braille | Braille 
| Bugi | Bugi 
| Buhid | Buhid 
| Aborígene\_Canadense | Aborígine Canadense 
| Cariano | Cariano 
| Chakma | Chakma 
| Cham | Cham 
| Cherokee | Cherokee | 
|Comum | Caracteres não específicos de um script 
| Cóptico | Cóptico 
| Cuneiforme | Cuneiforme 
| Cipriota | Cipriota 
| Cirílico | Cirílico 
| Deseret | Deseret 
| Devanágari | Devanágari 
| Hieróglifos\_Egípcios | Hieróglifos egípcios 
| Etíope | Etíope 
| Georgiano | Georgiano 
| Glagolítica | Glagolítica 
| Gothic | Gothic 
| Grego | Grego 
| Guzerate | Guzerate 
| Gurmuqui | Gurmuqui 
| Han | Han 
| Hangul | Hangul 
| Hanunoo | Hanunoo 
| Hebraico | Hebraico 
| Hiragana | Hiragana 
| Armi| Armi 
| Herdado | Script herdado do caractere anterior 
| Phli | Phli 
| Prti | Prti 
| Javanês | Javanês 
| Kaithi | Kaithi 
| Kannada | Kannada 
| Katakana | Katakana 
| Kayah\_Li | Li Kayah 
| Kharoshthi | Kharoshthi 
| Khmer | Khmer 
| Laosiano | Laosiano 
| Latino | Latino 
| Lepcha | Lepcha 
| Limbu | Limbu 
| Linear\_B | Linear B 
| Lycian | Lycian 
| Lídio | Lídio 
| Malaiala | Malaiala 
| Mandaico | Mandaico 
| Metei\_Mayek | Metei Mayek 
| Meroítico\_Cursivo | Meroítico Cursivo 
| Hieróglifos\_Meroíticos | Hieróglifos Meroíticos 
| Miao | Miao 
| Mongol | Mongol 
| Myanmar | Myanmar 
| Tai\_Lue\_Novo | Tai Lue Novo (também conhecido como Tai Lue simplificado) 
| Nko | Nko 
| Ogham | Ogham 
| Ol\_Chiki | OL Chiki 
| Itálico\_Antigo | Itálico antigo 
| Persa\_Antigo | Persa antigo 
| Árabe\_Meridional\_Antigo | Árabe Meridional Antigo 
| Turco\_Antigo | Turco antigo 
| Oriá | Oriá 
| Osmanya | Osmanya 
| Phags\_Pa | Phags Pa 
| Fenícia | Fenícia 
| Rejang | Rejang 
| Rúnica | Rúnica 
| Saurashtra | Saurashtra 
| Sharada | Sharada 
| Shavian | Shavian 
| Cingalês | Cingalês 
| Sora\_Sompeng | Sora Sompeng 
| Sundanês | Sundanês 
| Syloti\_Nagri | Syloti Nagri 
| Siríaco | Siríaco 
| Tagalo | Tagalo 
| Tagbanwa | Tagbanwa 
| Tai\_Le | Tai Le 
| Tai\_Tham | Tai Tham 
| Tai\_Viet | Tai Viet 
| Takri | Takri 
| Tâmil | Tâmil 
| Télugo | Télugo 
| Thaana | Thaana 
| Tailandês | Tailandês 
| Tibetano | Tibetano 
| Tifinagh | Tifinagh 
| Ugarítico | Ugarítico 
| Vai | Vai 
| Yi | Yi 
|Classes de caractere Vi: | 
|\\i |caractere identificador (SEM SUPORTE) VIM 
|\\I |\\i exceto dígitos (SEM SUPORTE) VIM 
|\\k | caractere de palavra-chave (SEM SUPORTE) VIM 
|\\K |\\k exceto dígitos (SEM SUPORTE) VIM 
|\\f | caractere de nome do arquivo (SEM SUPORTE) VIM 
|\\F |\\f exceto dígitos (SEM SUPORTE) VIM 
|\\p | caractere para impressão (SEM SUPORTE) VIM 
|\\P |\\p exceto dígitos (SEM SUPORTE) VIM 
|\\s |caractere de espaço em branco (== [ \\t]) (SEM SUPORTE) VIM 
|\\S | caractere não espaço em branco (== [^ \\t]) (SEM SUPORTE) VIM 
|\\d |dígitos (== [0-9]) VIM 
|\\D |not \\d VIM 
|\\x |dígitos hexadecimais (== [0-9A-Fa-f]) (SEM SUPORTE) VIM 
|\\X |not \\x (SEM SUPORTE) VIM 
|\\o |dígitos octais (== [0-7]) (SEM SUPORTE) VIM
|\\O |not \\o (SEM SUPORTE) VIM 
|\\w |caractere de palavra VIM 
|\\W |not \\w VIM 
|\\h |cabeça do caractere de palavra (SEM SUPORTE) VIM 
|\\H |not \\h (SEM SUPORTE) VIM 
|\\a |alfabético (SEM SUPORTE) VIM 
|\\A |not \\a (SEM SUPORTE) VIM 
|\\l |minúsculo (SEM SUPORTE) VIM 
|\\L |não minúsculo (SEM SUPORTE) VIM 
|\\u |maiúscula (SEM SUPORTE) VIM 
|\\U |não maiúscula (SEM SUPORTE) VIM 
|\_x |\\x mais nova linha, para qualquer x (SEM SUPORTE) VIM 
|sinalizadores VIM: | 
|\\c |ignorar maiúsculas/minúsculas (SEM SUPORTE) VIM 
|\\C |diferenciar maiúsculas/minúsculas (SEM SUPORTE) VIM 
|\\m |magic (SEM SUPORTE) VIM 
|\\M |nomagic (SEM SUPORTE) VIM 
|\\v |verymagic (SEM SUPORTE) VIM 
|\\V |verynomagic (SEM SUPORTE) VIM 
|\\Z |ignorar diferenças nos caracteres de combinação Unicode (SEM SUPORTE) VIM 
|Magic: | 
|(?{code}) |código Perl aleatório (SEM SUPORTE) PERL 
|(??{code}) |código Perl aleatório adiado (SEM SUPORTE) PERL 
|(?n) |chamada recursiva para grupo de captura regexp n (SEM SUPORTE) 
|(?+n) |chamada recursiva para grupo relativo +n (SEM SUPORTE) 
|(?-n) |chamada recursiva para grupo relativo -n (SEM SUPORTE) 
|(?C) |texto explicativo PCRE (SEM SUPORTE) PCRE 
|(?R) |chamada recursiva para todo regexp (== (?0)) (SEM SUPORTE) 
|(?&name) |chamada recursiva para grupo nomeado (SEM SUPORTE) 
|(?P=name) |referência inversa nomeada (SEM SUPORTE) 
|(?P>name) |chamada recursiva para grupo nomeado (SEM SUPORTE) 
|(?(cond)true&#124;false) |ramificação condicional (SEM SUPORTE) 
|(?(cond)true) |ramificação condicional (SEM SUPORTE) 
|(*ACCEPT) |tornar regexps mais como Prolog (SEM SUPORTE) 
|(*COMMIT) |(SEM SUPORTE) 
|(*F) |(SEM SUPORTE) 
|(*FAIL) |(SEM SUPORTE) 
|(*MARK) |(SEM SUPORTE) 
|(*PRUNE) |(SEM SUPORTE) 
|(*SKIP) |(SEM SUPORTE)
|(*THEN) |(SEM SUPORTE) 
|(*ANY) |definir convenção de nova linha (SEM SUPORTE) 
|(*ANYCRLF) |(SEM SUPORTE) 
|(*CR) |(SEM SUPORTE) 
|(*CRLF) |(SEM SUPORTE) 
|(*LF) |(SEM SUPORTE) 
|(*BSR\_ANYCRLF) | definir convenção \\R (SEM SUPORTE) PCRE 
|(*BSR\_UNICODE) |(SEM SUPORTE) PCRE




[AZURE.INCLUDE [app-analytics-footer](../../includes/app-analytics-footer.md)]

<!-----------HONumber=AcomDC_0330_2016-->