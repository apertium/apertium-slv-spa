all:	juice gatherdata meteor
juice:
	cat test.sl.txt | apertium -d ../ -f txt sl-es > translated.es.txt
gatherdata:
	./makeMeteorFormat src SL ES test.sl.txt  |tr '[:upper:]' '[:lower:]' > src
	./makeMeteorFormat tst SL ES translated.es.txt | tr '[:upper:]' '[:lower:]'> tst
	./makeMeteorFormat ref SL ES test.es.txt | tr '[:upper:]' '[:lower:]' >ref
meteor:
	java -Xmx1G -jar meteor-*.jar tst ref -normalize   -keepPunctuation -writeAlignments -l es > results_meteor_sl-es.txt
	touch results_meteor_timeline
	date >> results_meteor_timeline
	tail results_meteor_sl-es.txt >> results_meteor_timeline
clean:
	rm -f  src ref tst results_meteor_timeline meteor-align.out translated.es.txt results_meteor_sl-es.txt
