#include <stdio.h>
#include <stdlib.h>
#include <ctype.h>
#include <string.h>
#include <Windows.h>
#include <time.h>

struct Datum {
    int dan, mesec, godina;
};

struct Akaunt {
    char ime[15];
    char prezime[15];
    int acc_id;
    float balans;
    struct Datum datumrodjenja;
};

struct Akaunt *akaunti=NULL; // niz struktura je nedefinisana na pocetku;
int n = 0; // broj korisnika je na pocetku 0;

void menidatoteka(char *imedatoteke);
void validacijaid(int *id);
int generisiaccid();
void noviakaunt();
void obrisiakaunt();
void depozit();
void withdraw();
void lista();
void detalji();
void menu();
void validacijadatuma(struct Datum *datum);
void azuriranje();
void validacijaimena(struct Akaunt *akaunt);
void validacijaprezimena(struct Akaunt *akaunt);
void vratinameni();

int main() {
	char dobrodosli[]="\t\t\t---> DOBRODOSLI <---";
	int length = strlen(dobrodosli);
	int i;
		remove("izvestaj.txt");
	for (i=0;i<length;i++)
	{
		printf("%c",dobrodosli[i]);
		Sleep(100);
	}
	printf("\n");
	srand(time(NULL));

    while (1) {
		
        menu();
        
    }

}
//funkcija za generisanje nasumicnog id-a korisnika
int generisiaccid() {
    int i;
    int acc_id = 0;
    for (i = 0; i < 5; i++) {
        acc_id = acc_id * 10 + rand();
    }
    return acc_id;
}
// funkcija za validaciju datuma rodjenja korisnika ( korisnik ne moze biti mladji od 18 godina )
void validacijadatuma(struct Datum *datum) {
    int daniumesecu[] = {0,31,28,31,30,31,30,31,31,30,31,30,31};

    do {
        printf("\n\nDatum rodjenja(dd mm yyyy): ");
        
		if(scanf("%d %d %d",&datum->dan,&datum->mesec,&datum->godina)!=3)
		{
			printf("\nNeispravan format datuma, pokusajte ponovo.\n");
			
			while(getchar() !='\n')
			{

			}
			continue;
		}

        if (datum->mesec < 1 || datum->mesec > 12) {
            printf("\nMesec mora biti u opsegu 1 i 12, pokusajte ponovo.\n");
            continue;
        }
        if (datum->dan < 1 || datum->dan > daniumesecu[datum->mesec]) {
            printf("\nNeispravan dan za dati mesec, pokusajte ponovo.\n");
            continue;
        }
        if (datum->godina < 1900 || datum->godina > 2005) { // 2005 - minimum 18 god.
            printf("\nNeispravan unos godine, pokusajte ponovo.\n");
            continue;
        }

        break;

    } while(1);
}
// funkcija za validaciju imena korisnika
void validacijaimena(struct Akaunt *akaunt) {
    int i, flag;

    do {
        flag = 1;
        printf("\nIme: ");
        scanf("%s", akaunt->ime);
        if (strlen(akaunt->ime) == 0) {
            printf("\nIme ne sme biti prazno, pokusajte ponovo.\n");
            flag = 0;
            continue;
        }

        for (i = 0; akaunt->ime[i] != '\0'; i++) {
            if (!isalpha(akaunt->ime[i]) && !isspace(akaunt->ime[i])) {
                printf("\nIme mora da sadrzi samo slova, pokusajte ponovo.\n");
                flag = 0;
                break;
            }
        }

    } while (!flag);
}
// funkcija za validaciju prezimena korisnika
void validacijaprezimena(struct Akaunt *akaunt) {
    int i, flag;

    do {
        flag = 1;
        printf("\nPrezime: ");
        scanf("%s", akaunt->prezime);

        if (strlen(akaunt->prezime) == 0) {
            printf("\nPrezime ne sme biti prazno, pokusajte ponovo.\n");
            flag = 0;
            continue;
        }

        for (i = 0; akaunt->prezime[i] != '\0'; i++) {
            if (!isalpha(akaunt->prezime[i]) && !isspace(akaunt->prezime[i])) {
                printf("\nPrezime mora da sadrzi samo slova, pokusajte ponovo.\n");
                flag = 0;
                break;
            }
        }

    } while (!flag);
}
// funkcija za validaciju id-a korisnika
void validacijaid(int *id)
{
	char input[15];
	int i,flag;

	do
	{
		flag=1;
		printf("\nID: ");
		scanf("%s",input);
		getchar();

		if(strlen(input)==0)
		{
			printf("\nId ne sme biti prazan, pokusajte ponovo.\n");
            flag = 0;
            continue;
		}

		for (i = 0; input[i] != '\0'; i++) {
            if (!isdigit(input[i]) && !isspace(input[i])) {
                printf("\nID mora da sadrzi samo brojeve, pokusajte ponovo.\n");
                flag = 0;
                break;
            }
        }

		*id = atoi(input);

	}while(!flag);

}

//funkcija koja sa delay-om vraca na glavni meni
void vratinameni()
{
	printf("\nVracanje na meni ."); 	Sleep(500);
	printf("."); 	Sleep(500);
	printf("."); 	Sleep(500);
	system("cls");
}

// funkcija za kreiranje novog akaunta realokacijom memorije
void noviakaunt() {
	FILE *fptr;
    system("cls");
    printf("\tKREIRANJE AKAUNTA\n\n");

        akaunti = (struct Akaunt*)realloc(akaunti, (n + 1) * sizeof(struct Akaunt));
        if (akaunti == NULL) {
            printf("\nGreska pri alokaciji memorije za akaunt.\n");
            exit(1);
        }

	akaunti[n].acc_id = generisiaccid();
    akaunti[n].balans = 1000;

    //akaunti[n].ime[0] = '\0';
    //akaunti[n].prezime[0] = '\0';

    //akaunti[n].datumrodjenja.dan = 0;
    //akaunti[n].datumrodjenja.mesec = 0;
    //akaunti[n].datumrodjenja.godina = 0;

    validacijaimena(&akaunti[n]);
    getchar();
    validacijaprezimena(&akaunti[n]);
    getchar();
    validacijadatuma(&akaunti[n].datumrodjenja);
    getchar();


	fptr=fopen("izvestaj.txt","a");
	if(fptr==NULL)
	{
		printf("\nGreska pri otvaranju datoteke.\n");
		exit(1);
	}
	
	fprintf(fptr,"\t\t\t\tIZVESTAJ\n");
	fprintf(fptr,"\n\nKREIRANJE AKAUNTA\n");
	fprintf(fptr,"ID: %d\nIme: %s\nPrezime: %s\nBalans: %.2f\nDatum rodjenja: %d.%d.%d\n",akaunti[n].acc_id,akaunti[n].ime,akaunti[n].prezime,akaunti[n].balans,akaunti[n].datumrodjenja.dan,akaunti[n].datumrodjenja.mesec,akaunti[n].datumrodjenja.godina);
	fclose(fptr);
    n++;

    printf("\n\nUspesno kreiran akaunt!\n");

	vratinameni();
    return;
}

//funkcija za brisanje akaunta iz memorije ( shiftuje elemente niza u levo, odnosno prepisuje se preko elementa na indeksu j
//sa elementom sa indeksom j+1
void obrisiakaunt() {
	FILE *fptr;
    int id,i, j, flag = -1,pogresanunos=0;
	  system("cls");

	  fptr=fopen("izvestaj.txt","a");
	  if(fptr==NULL)
	  {
		  printf("\nGreska pri otvaranju datoteke.\n");
		  exit(1);
	  }

   do
   {
	    printf("\nAkaunt id za brisanje: ");
    validacijaid(&id);

	if(id == 1)
	{
		vratinameni();
		return;
	}

    for (i = 0; i < n; i++) {
        if (akaunti[i].acc_id == id) {
            flag = i;
            break;
        }
    }

    if (flag != -1) {
        for (j = flag; j < n - 1; j++) {
            akaunti[j] = akaunti[j + 1];
        }
		pogresanunos=0;
        n--;
        printf("\nAkaunt uspesno obrisan.\n");

		fprintf(fptr,"\n\nBRISANJE NALOGA\n");
		fprintf(fptr,"Uspesno obrisan akaunt %d.",id);
		fclose(fptr);
		
	vratinameni();
    return;

    } else {
        printf("\nNije pronadjen akaunt sa ovim id-om.\n");
		pogresanunos++;
		
		if(pogresanunos==2)
		{
			printf("\nPogresili ste ID vec 2 puta,unesite 1 ako zelite nazad na meni.\n");
		}

    }
   }while(flag==-1);
}
//funkcija za depozit balansa na korisnikov nalog 
void depozit() {
	FILE *fptr;
    float deposit;
    int id, i, flag = -1,pogresanunos=0;
	system("cls");
	 fptr=fopen("izvestaj.txt","a");
	  if(fptr==NULL)
	  {
		  printf("\nGreska pri otvaranju datoteke.\n");
		  exit(1);
	  }

	do
	{
		printf("\n\nNa koji akaunt zelite da izvrsite depozit? ");
    validacijaid(&id);

	if(id == 1)
	{
		vratinameni();
		return;
	}

    for (i = 0; i < n; i++) {
        if (akaunti[i].acc_id == id) {
            flag = i;
            break;
        }
    }
    if (flag != -1) {
		pogresanunos=0;
        do {
			printf("\nAkaunt pronadjen!\n");
            printf("\n\nKoliki depozit zelite da izvrsite? ");
            scanf("%f", &deposit);
            if (deposit <= 0) {
                printf("\n\nDeposit mora biti veci od 0, pokusajte ponovo.\n");
            }
        } while (deposit <= 0);

        akaunti[flag].balans += deposit;
        printf("\n\nDepozit od %.2f na akaunt %d je uspesno izvrsen.\n", deposit, id);

		fprintf(fptr,"\n\nDEPOZIT\n");
		fprintf(fptr,"Uspesno izvrsen depozit od %.2f na akaunt %d.\n",deposit,id);
		fclose(fptr);

		vratinameni();
		return;

    } else {
        printf("\nNije pronadjen akaunt sa ovim id-om.\n");
		pogresanunos++;
		
		if(pogresanunos==2)
		{
			printf("\nPogresili ste ID vec 2 puta,unesite 1 ako zelite nazad na meni.\n");
		}
    }
	}while(flag==-1);
    
}
//funkcija za podizanje novca sa korisnikvog naloga
void withdraw() {
	FILE *fptr;
    int id, i, flag = -1,pogresanunos=0,sredstva=-1;
    float withdraw;
	system("cls");

	fptr=fopen("izvestaj.txt","a");
	  if(fptr==NULL)
	  {
		  printf("\nGreska pri otvaranju datoteke.\n");
		  exit(1);
	  }

   do
   {
	printf("\n\nNa koji akaunt zelite da izvrsite withdraw? ");
    validacijaid(&id);

	if(id == 1)
	{
		vratinameni();
		return;
	}

    for (i = 0; i < n; i++) {
        if (akaunti[i].acc_id == id) {
            flag = i;
            break;
        }
    }
    if (flag != -1) {
		pogresanunos=0;
       do
	   {
		    do {
            printf("\n\nKoliki withdraw zelite da izvrsite? ");
            scanf("%f", &withdraw);
            if (withdraw <= 0) {
                printf("\n\nWithdraw mora biti veci od 0.\n");
            }
        } while (withdraw <= 0);
        if (akaunti[flag].balans >= withdraw) {
			sredstva=1;
            akaunti[flag].balans -= withdraw;
            printf("\n\nWithdraw od %.2f na akaunt %d je uspesno izvrsen.\n", withdraw, id);

			fprintf(fptr,"\n\nWITHDRAW\n");
		fprintf(fptr,"Uspesno izvrsen withdraw od %.2f na akaunt %d.\n",withdraw,id);
		fclose(fptr);

		vratinameni();
		return;
		system("cls");

        } else {
            printf("\n\nNemate dovoljno sredstava na racunu! Pokusajte ponovo.\n");
        }
	   }while(sredstva==-1);
    } else {
        printf("\nNije pronadjen akaunt sa ovim id-om.\n");
		pogresanunos++;
			if(pogresanunos==2)
			{
				printf("\nPogresili ste ID vec 2 puta,unesite 1 ako zelite nazad na meni.\n");
			}
    }
   }while(flag==-1);
}
//funkcija koja prikazuje listu svih naloga u memoriji
void lista() {
    int i,opcija=0;
	system("cls");
    printf("\n\n\t\tLISTA NALOGA\t\t\n\n");
    printf("IME\tPREZIME\t\t ID\t\tBALANS\t\tDATUM_RODJENJA");
    for (i = 0; i < n; i++) {
		printf("\n\n%s\t%s\t%d\t%.2f\t\t%d.%d.%d\n", akaunti[i].ime, akaunti[i].prezime, akaunti[i].acc_id,
               akaunti[i].balans, akaunti[i].datumrodjenja.dan, akaunti[i].datumrodjenja.mesec,
               akaunti[i].datumrodjenja.godina);
    }
	Sleep(1500);
	do
	{
		printf("\n\nUnesite 1 za vracanje nazad u meni.\n");
	    scanf("%d",&opcija);
		if(opcija==1)
	{
		vratinameni();
		return;
	}
	else
	{
		printf("\nNeispravan unos! Pokusajte ponovo.\n");
	}
	}while(opcija!=1);
}


//funkcija za azuriranje podataka nekog od korisnika, pristupamo preko korisnikovog id-a ili imena
void azuriranje() {
   FILE *fptr;
   int flag=-1,id,i,pogresanunos=0;
   system("cls");
   fptr=fopen("izvestaj.txt","a");
   if(fptr==NULL)
   {
	   printf("\nGreska pri otvaranju datoteke.\n");
	   exit(1);
   }

   do
   {
	   printf("\nAkaunt ID za azuriranje: ");
   validacijaid(&id);

   if(id == 1)
	{
		vratinameni();
		return;
	}

   for(i=0;i<n;i++)
   {
	   if(akaunti[i].acc_id==id)
	   {
		   flag=i;
		   break;
	   }
   }

   if(flag!=-1)
   {
	   pogresanunos=0;
	    printf("\nTrenutni detalji naloga sa ID %d:\n\n", id);
        printf("Ime: %s\nPrezime: %s\nBalans: %.2f\nDatum rodjenja: %d.%d.%d\n", akaunti[flag].ime, akaunti[flag].prezime,
        akaunti[flag].balans, akaunti[flag].datumrodjenja.dan,
        akaunti[flag].datumrodjenja.mesec, akaunti[flag].datumrodjenja.godina); 

	    akaunti[flag].ime[0] = '\0';
        akaunti[flag].prezime[0] = '\0';
        akaunti[flag].datumrodjenja.dan = 0;
        akaunti[flag].datumrodjenja.mesec = 0;
        akaunti[flag].datumrodjenja.godina = 0;

		printf("\nUnos novih podataka: \n\n");

	   validacijaimena(&akaunti[flag]);
       validacijaprezimena(&akaunti[flag]);
	   validacijadatuma(&akaunti[flag].datumrodjenja);

	   printf("\nUspesno azuriranje akaunta %d.\n",id);

	   fprintf(fptr,"\n\nAZURIRANJE\n");
       fprintf(fptr, "Novi detalji naloga %d:\n",id);
       fprintf(fptr, "Ime: %s\nPrezime: %s\nBalans: %.2f\nDatum rodjenja: %d.%d.%d\n", akaunti[flag].ime, akaunti[flag].prezime,
       akaunti[flag].balans, akaunti[flag].datumrodjenja.dan,
       akaunti[flag].datumrodjenja.mesec, akaunti[flag].datumrodjenja.godina);

	    vratinameni();
		return;
		system("cls");

   }
   else
   {
	    printf("\nNije pronadjen akaunt sa ovim id-om.\n");
		pogresanunos++;
		if(pogresanunos==2)
		{
			printf("\nPogresili ste ID vec 2 puta,unesite 1 ako zelite nazad na meni.\n");
		}
		
   }
   }while(flag==-1);

}
//funkcija za detaljan prikaz podataka nekog od naloga
void detalji() {
    int id, flag = -1, i, opcija,pogresanunos=0;
	struct Akaunt temp; // dodao sam zbog validacije imena u case 2 da ne bih pravio ponovo posebnu funkciju;
	system("cls");
    printf("\n\nPodatke o nalogu zelite pregledati po:\n");
    printf("\n[1] Id-u\n");
    printf("\n[2] Imenu\n");

    do {
        printf("\n\n(Izaberite opciju 1 ili 2): ");
        scanf("%d", &opcija);

        switch (opcija) {
            case 1:
				system("cls");
              do
			  {
				printf("\n\nId akaunta: ");
                validacijaid(&id);

				if(id == 1)
	{
		vratinameni();
		return;
	}

                for (i = 0; i < n; i++) {
                    if (akaunti[i].acc_id == id) {
                        flag = i;
                        break;
                    }
                }

                if (flag != -1) {
					system("cls");
					pogresanunos=0;
                    printf("\nDetalji naloga sa ID %d:\n\n", id);
                    printf("%s\n%s\n%.2f\n%d.%d.%d\n", akaunti[flag].ime, akaunti[flag].prezime,
                           akaunti[flag].balans, akaunti[flag].datumrodjenja.dan,
                           akaunti[flag].datumrodjenja.mesec, akaunti[flag].datumrodjenja.godina);
                } else {
                    printf("\nNepostoji akaunt sa ovim id-om.\n");
					pogresanunos++;
					if(pogresanunos==2)
					{
						printf("\nUneli ste pogresan unos vec 2 puta, unesite 1 za vracanje na meni.\n");
					}
                }
			  }while(flag==-1);
                break;

            case 2:
				system("cls");
               do
			   {
				printf("\n\nIme akaunta: ");
                validacijaimena(&temp);// ono o cemu sam pricao pre za validaciju sa struct Akaunt temp;
                                for (i = 0; i < n; i++) {
                    if (strcmp(akaunti[i].ime, temp.ime) == 0) {
                        flag = i;
                        break;
                    }
                }

                if (flag != -1) {
					system("cls");
					pogresanunos=0;
                    printf("\nDetalji naloga sa imenom %s:\n\n", temp.ime);
                    printf("%s\n%s\n%.2f\n%d.%d.%d\n", akaunti[flag].ime, akaunti[flag].prezime,
                           akaunti[flag].balans, akaunti[flag].datumrodjenja.dan,
                           akaunti[flag].datumrodjenja.mesec, akaunti[flag].datumrodjenja.godina);

					Sleep(1500);
				do {
					printf("\n\nUnesite 1 za povratak nazad u meni.\n");
					scanf("%d", &opcija);

					if (opcija == 1) {
						vratinameni();
						return;
					} else {
						printf("\nNeispravan unos! Pokusajte ponovo.\n");
					}
				} while (opcija != 1);


                } else {
                    printf("\nNepostoji akaunt sa ovim imenom.\n");
					pogresanunos++;
					if(pogresanunos==2)
					{
						printf("\nUneli ste pogresan unos vec 2 puta, unesite 1 za vracanje na meni.\n");
					}
                }
			   }while(flag==-1);
                break;

            default:
                printf("\nNiste izabrali validnu opciju. Molimo vas da izaberete opciju 1 ili 2.\n");
        }

    } while (opcija != 1 && opcija != 2);
}
//funkcija za citanje menija iz datoteke
void menidatoteka(char *imedatoteke)
{
	FILE *fptr;
	char string[100];

	fptr=fopen(imedatoteke,"r");

	if(fptr==NULL)
	{
		printf("\nGreska pri otvaranju menija iz datoteke.\n");
		exit(1);
	}

	while(fgets(string,sizeof(string),fptr)!=NULL)
	{
		printf("%s",string);
	}
	fclose(fptr);
}
//funkcija koja sluzi kao odabir neke od svih navedenih funkcija, sa obzirom ako nema korisnika da automatski
//poziva funkciju za kreiranje naloga
void menu() {
    int izbor=0;

    if (n == 0) {
       printf("\nNema dostupnih akaunta. Ubrzo cete imati mogucnost da kreirate jedan.");
	   Sleep(1200);
	   printf(" .");
	   Sleep(1200);
	   printf(" .");
	   Sleep(1200);
	   printf(" .");
       noviakaunt();
    } else {

		menidatoteka("meni.txt");

        printf("\n\n\tIzaberite opciju (1-8): ");
        scanf("%d", &izbor);
    }

    switch (izbor) {
        case 1:
            noviakaunt();
            break;

        case 2:
            obrisiakaunt();
            break;

        case 3:
            depozit();
            break;

        case 4:
            withdraw();
            break;

        case 5:
            lista();
            break;

        case 6:
            detalji();
            break;

        case 7:
            azuriranje();
            break;

        case 8:
            printf("\nIzlaz iz programa.\n");
			if (akaunti != NULL) {
            free(akaunti);
        }
            exit(1);

		case 0:
			break;

        default:
            printf("\nNiste izabrali validnu opciju. Molimo vas da izaberete opciju od 1 do 8.\n");
    }
}