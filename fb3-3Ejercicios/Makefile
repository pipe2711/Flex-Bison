fb3-3: fb3-3.l fb3-3.y fb3-3.h
	bison -d fb3-3.y
	flex -ofb3-3.lex.c fb3-3.l
	cc -o $@ fb3-3.tab.c fb3-3.lex.c fb3-3funcs.c -lm

clean:
	rm fb3-3.tab.c fb3-3.tab.h fb3-3.lex.c fb3-3
