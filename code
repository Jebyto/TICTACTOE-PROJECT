#include <iostream>
#include <stdlib.h>
#include <stdio.h>
using namespace std;
//Criado por José Carlos Mota Matias Feitosa
typedef struct inteligenciaArtificial
{

    int tabuleiro[3][3];// o tabuleiro da arvore, onde pode ter inumeras possibilidades de jogada
    struct inteligenciaArtificial* possib[9]; //as possibilidades, voce tem 9 possibilidades de jogada, e vai diminuindo em 1 a cada turno
    int max; //positivo caso o caminho seja melhor para o jogador max (X) ou negativo para o mini (O), variavel que define a inteligencia artificial
    int turno; //o turno onde este no esta

} IA;

bool checa1(int tabuleiro[3][3]);//funcao que checa se o primeiro ganhou
bool checa2(int tabuleiro[3][3]);//funcao que checa se o segundo ganhou
bool adiciona(int tabuleiro[3][3], int lugar, int jogador);//funcao que adiciona uma peca no tabuleiro
void cria(int tabuleiro[3][3]);

bool compara(int tab1[3][3],int tab2[3][3])
{

    for(int i = 0; i<3; i++)
    {
        for(int j = 0; j<3; j++)
        {
            if(tab1[i][j]!=tab2[i][j])
                return false;
        }
    }
    return true;
}

void criarpossibilidades(IA ** raiz, int tab[3][3], int max, int jogo) //funcao que cria todas as possibilidades de jogo
{
    int jog;
    IA* novo = (IA*) malloc(sizeof(IA));
    novo->max = 0;
    novo->turno = jogo;
    for(int i = 0; i<3; i++)
    {
        for(int j = 0; j<3; j++)
        {
            novo->tabuleiro[i][j] = tab[i][j];
        }
    }
    for(int i = 0; i<9; i++)
        novo->possib[i]=NULL;
    *raiz = novo;
    if(jogo == 1)
        jog = 2;
    if(jogo == 2||jogo==0)
        jog = 1;
    int aux[3][3];
    if(checa1(tab)||checa2(tab))
        return;
    for(int i = 0; i<9; i++)
    {
        for(int j = 0; j<3; j++)
        {
            for(int k = 0; k<3; k++)
            {
                aux[j][k] = tab[j][k];
            }
        }
        if(adiciona(aux,i+1,jog))
        {
            criarpossibilidades(&(*raiz)->possib[i], aux, 0, jog);
        }
    }
}
int caminho(IA* raiz)  // escreve as chances de vitoria de um caminho
{
    if(raiz==NULL)
        return 0;

    int filho=0;
    for(int i = 0; i<9; i++)
    {
        if(raiz->possib[i]==NULL)
            filho++;
    }
    if(filho==9)
        return raiz->max;
    for(int i = 0; i<9; i++)
        return (caminho(raiz->possib[i]) + raiz->max);

}

void minimax(IA** raiz, bool reseta)  //calcula o max que é a variavel que define a inteligencia, se reseta for true ele cria tudo denovo, se for false ele adiciona as possibilidades dependendo das vitorias seguindo ela
{
    if(*raiz==NULL)
        return;
    int filho=0;
    for(int i = 0; i<9; i++)
    {
        if((*raiz)->possib[i]==NULL)
            filho++;
    }
    if(filho==9)
    {
        if(reseta)
        {
            if(checa1((*raiz)->tabuleiro))
            {
                (*raiz)->max = 1;
            }
            else if(checa2((*raiz)->tabuleiro))
            {
                (*raiz)->max = -1;
            }
            else
            {
                (*raiz)->max = 0;
            }
        }
        return;
    }
    for(int i = 0; i<9; i++)
        minimax(&(*raiz)->possib[i],reseta);

    if((*raiz)->turno == 1)
    {
        //int ma=-1;
        for(int i = 0; i<9; i++)
        {
            if((*raiz)->possib[i]!=NULL)
            {
                (*raiz)->max += (*raiz)->possib[i]->max;
            }
        }
    }
    if((*raiz)->turno == 2)
    {
        for(int i = 0; i<9; i++)
        {
            if((*raiz)->possib[i]!=NULL)
            {
                (*raiz)->max += (*raiz)->possib[i]->max;
            }
        }
    }

}

int inteligencia(IA* raiz, int jogador, int dificuldade)  // a funcao que faz a IA funcionar, dizendo onde ela deve seguir
{
    if(raiz==NULL) //se for nulo nao retorna nada "0"
        return 0;
    int filho=0;
    for(int i = 0; i<9; i++)
    {
        if((raiz)->possib[i]==NULL)
            filho++;
    }
    if(filho==9) //e nem se for um no folha retorna
        return 0;

    int rdm = rand()%10+1; //calcula a chance de ser uma jogada boa
    int chance =  dificuldade*dificuldade+2; // dependendo da dificuldade, a chance pode ser baixa, ou pode ser 100%
    int burra = rand()%9;// se a jogada nao for boa, a IA fará uma jogada burra
    if(rdm>=chance){
            while(raiz->possib[burra]==NULL)
        {
        burra = rand()%9;
        }
        return burra;
    }

    int bug[3][3]; //tem 2 casos específicos que só dão para resolver assim
    cria(bug);
    adiciona(bug,1,1);
    adiciona(bug,9,1);
    adiciona(bug,5,2);
    if(compara(bug,raiz->tabuleiro))
        return 5;
    cria(bug);
    adiciona(bug,3,1);
    adiciona(bug,7,1);
    adiciona(bug,5,2);
    if(compara(bug,raiz->tabuleiro))
        return 3;

    int cam = 0; //caminho que deve seguir
    int val; //pega o valor maximo do caminho
    bool nao; //se o jogador for ganhar em alguma jogada, a IA "nao" vai deixar
    if(jogador == 1)
    {
        int contador = 0; // vai contar quantas vezes pode perder no futuro
        int encurralado= -1;// vai verificar se está encurralado e se estiver salva o valor aonde vai ser
        int derrota=-1;//salva o valor onde pode perder
        for(int i = 0; i<9; i++)
        {
            if(raiz->possib[i]!=NULL)
                if(checa1(raiz->possib[i]->tabuleiro))//se ele já for ganhar, já retorna o valor
                    return i;
            for(int j = 0; j<9; j++)
            {
                if(raiz->possib[i]!=NULL&&raiz->possib[i]->possib[j]!=NULL)
                    if(checa2(raiz->possib[i]->possib[j]->tabuleiro)==true)// se ele for perder, ele salva aonde
                        derrota = j;
                for(int k = 0; k<9; k++)
                {
                    contador = 0;
                    for(int l = 0; l<9; l++)
                    {
                        if(raiz->possib[i]!=NULL&&raiz->possib[i]->possib[j]!=NULL&&raiz->possib[i]->possib[j]->possib[k]!=NULL&&raiz->possib[i]->possib[j]->possib[k]->possib[l]!=NULL)
                        {
                            if(checa2(raiz->possib[i]->possib[j]->possib[k]->possib[l]->tabuleiro))// verifica quantas vezes ele vai perder no futuro
                                contador++;
                            if(contador >= 2) // se ele perder 2 vezes ou mais ele está encurralado
                            {
                                encurralado = j;
                            }
                        }
                    }
                }
            }
        }
        if(derrota!=-1)// se ele for perder em um canto ele retorna onde vai ser para impedir a jogada
            return derrota;
        for(int i = 0; i<9; i++)
        {
            nao=false;
            if(raiz->possib[i]!=NULL)
            {
                if(checa1(raiz->possib[i]->tabuleiro))//se a inteligencia for ganhar ela nao precisa seguir a arvore, gasta botar onde ganha
                    return i;
                if(caminho(raiz->possib[i])>=val)//vai rodando ate pegar o melhor valor de val e o cam vai pegar o caminho desse maior valor
                {
                    for(int j = 0; j<9; j++)//essa funcao olha a possibilidade da possibilidade para deixar a inteligencia melhor ainda
                    {
                        if(raiz->possib[i]->possib[j]!=NULL)//a possib da possib nao pode ser nula tambem
                            if(checa2(raiz->possib[i]->possib[j]->tabuleiro))//se em alguma possibilidade da possibilidade o jogador for ganha a IA nao deixa
                            {
                                nao = true;
                                break;
                            }
                    }
                    if(nao==false)
                    {
                        val = caminho(raiz->possib[i]);//se o caminho for bom a IA anota
                        cam = i;
                    }
                }
            }
        }
        nao = false;
        for(int j = 0; j<9; j++)// ultima checagem para ver se a jogada não vai deixar o jogador ganhar
        {
            if(raiz->possib[cam]->possib[j]!=NULL)
                if(checa2(raiz->possib[cam]->possib[j]->tabuleiro))
                {
                    nao = true;
                    break;
                }
        }
        if(encurralado!=-1&&nao==false) // se a jogada o jogador não for ganhar agora, mas está planegando encurralar, a IA não deixa
        {
            return encurralado;
        }
        else
        {
            return cam;
        }//se der tudo certo, então pode retornar

    }
    if(jogador == 2)  //mesma coisa do de cima, mas para o jogador 2
    {
        int contador = 0;
        int encurralado= -1;
        int derrota=-1;
        for(int i = 0; i<9; i++)
        {
            if(raiz->possib[i]!=NULL)
                if(checa2(raiz->possib[i]->tabuleiro))
                    return i;
            for(int j = 0; j<9; j++)
            {
                if(raiz->possib[i]!=NULL&&raiz->possib[i]->possib[j]!=NULL)
                    if(checa1(raiz->possib[i]->possib[j]->tabuleiro)==true)
                        derrota = j;
                for(int k = 0; k<9; k++)
                {
                    contador = 0;
                    for(int l = 0; l<9; l++)
                    {
                        if(raiz->possib[i]!=NULL&&raiz->possib[i]->possib[j]!=NULL&&raiz->possib[i]->possib[j]->possib[k]!=NULL&&raiz->possib[i]->possib[j]->possib[k]->possib[l]!=NULL)
                        {
                            if(checa1(raiz->possib[i]->possib[j]->possib[k]->possib[l]->tabuleiro))
                                contador++;
                            if(contador >= 2)
                            {
                                encurralado = j;
                            }
                        }
                    }
                }
            }
        }
        if(derrota!=-1)
            return derrota;
        for(int i = 0; i<9; i++)
        {
            nao=false;
            if(raiz->possib[i]!=NULL)
            {
                if(checa2(raiz->possib[i]->tabuleiro))
                    return i;
                if(caminho(raiz->possib[i])<=val)
                {
                    for(int j = 0; j<9; j++)
                    {
                        if(raiz->possib[i]->possib[j]!=NULL)
                            if(checa1(raiz->possib[i]->possib[j]->tabuleiro))
                            {
                                nao = true;
                                break;
                            }
                    }
                    if(nao==false)
                    {
                        val = caminho(raiz->possib[i]);
                        cam = i;
                    }
                }
            }
        }
        nao = false;
        for(int j = 0; j<9; j++)
        {
            if(raiz->possib[cam]->possib[j]!=NULL)
                if(checa1(raiz->possib[cam]->possib[j]->tabuleiro))
                {
                    nao = true;
                    break;
                }
        }
        if(encurralado!=-1&&nao==false)
        {
            return encurralado;
        }
        else
        {
            return cam;
        }

    }
    //no final de tudo vai retornar o melhor caminho

}
void desenha(int tabuleiro[3][3])  //desenha o tabuleiro
{
    int k = 1;
    for(int i = 0; i<3; i++)
    {
        for(int j = 0; j<3; j++)
        {
            if(j==0)
                cout<<"       ";
            if(tabuleiro[i][j]==0)
                cout<<k;
            if(tabuleiro[i][j]==1)
                cout<<"X";
            if(tabuleiro[i][j]==2)
                cout<<"O";
            if(j!=2)
                cout<<"|";
            k++;
        }
        if(i!=2)
            cout<<"\n       -----"<<endl;
        if(i==2)
            cout<<""<<endl;
    }
}

bool adiciona(int tabuleiro[3][3], int lugar, int jogador)
{
    int k = 1;
    for(int i = 0; i<3; i++)
    {
        for(int j = 0; j<3; j++)
        {
            if(lugar == k)
            {
                if(tabuleiro[i][j]==0)
                {
                    tabuleiro[i][j] = jogador;
                    return true;
                }
                else
                {

                    return false;
                }
            }
            k++;
        }
    }
    return false;
}
void cria(int tabuleiro[3][3])  //cria o tabuleiro
{
    for(int i = 0; i<3; i++)
    {
        for(int j = 0; j<3; j++)
        {
            tabuleiro[i][j] = 0;
        }
    }
}

bool checa1(int tabuleiro[3][3])
{
    for(int i=0; i<3; i++)
    {
        if(tabuleiro[i][0]==1 && tabuleiro[i][2]==1 && tabuleiro[i][1]==1)
            return true;
    }
    for(int j=0; j<3; j++)
    {
        if(tabuleiro[0][j]==1 && tabuleiro[1][j]==1 && tabuleiro[2][j]==1)
            return true;
    }


    if(tabuleiro[0][0]==tabuleiro[1][1] && tabuleiro[1][1]==tabuleiro[2][2] && tabuleiro[0][0]==1)
        return true;
    if(tabuleiro[0][2]==tabuleiro[1][1] && tabuleiro[1][1]==tabuleiro[2][0] && tabuleiro[2][0]==1)
        return true;

    return false;
}
bool checa2(int tabuleiro[3][3])
{
    for(int i=0; i<3; i++)
    {
        if(tabuleiro[i][0]==2 && tabuleiro[i][2]==2 && tabuleiro[i][1]==2)
            return true;
    }
    for(int j=0; j<3; j++)
    {
        if(tabuleiro[0][j]==2 && tabuleiro[1][j]==2 && tabuleiro[2][j]==2)
            return true;
    }


    if(tabuleiro[0][0]==tabuleiro[1][1] && tabuleiro[1][1]==tabuleiro[2][2] && tabuleiro[0][0]==2)
        return true;
    if(tabuleiro[0][2]==tabuleiro[1][1] && tabuleiro[1][1]==tabuleiro[2][0] && tabuleiro[2][0]==2)
        return true;

    return false;
}

int main()
{
    int tabuleiro[3][3];
    cria(tabuleiro);
    IA* possib = NULL;
    IA* aux = NULL;
    criarpossibilidades(&possib,tabuleiro, 0, 2);
    minimax(&possib,true);
    aux = possib;
    int jogada;
    int jogador=1;
    int dificuldade;
    int ajuda = rand()%9; // adiciona em qualquer canto caso a IA no medio ou facil nao consiga jogar
    cout << "\nDiga qual a dificuldade voce quer jogar, 1 para facil, 2 para medio, 3 para impossivel"<<endl;
    cin >> dificuldade;
    cin.clear();
    cin.ignore();
    while(dificuldade<1||dificuldade>3){
            cout << "\n Resposta invalida, diga qual a dificuldade voce quer jogar, 1 para facil, 2 para medio, 3 para impossivel"<<endl;
            cin >> dificuldade;
            cin.clear();
            cin.ignore();
            }
    while(true){//roda o progama infinitamente
        system("cls");
        cout<<"\n\n"<<endl;
        desenha(aux->tabuleiro);
        if(jogador==1)  //vez do jogador 1
        {
            cin >> jogada; //o jogador escolhe
            cin.clear();
            cin.ignore();
            if(adiciona(tabuleiro,jogada,jogador))
            {
                jogador=2;
                if(aux->possib[jogada-1]!=NULL)
                    aux = aux->possib[jogada-1];
            }
        }
        else
        {
            jogada = inteligencia(aux,2,dificuldade); //IA joga
            if(adiciona(tabuleiro,1+jogada,jogador)==false){ //olha se da para adicionar e adiciona se der
                while(aux->possib[ajuda]==NULL)
        {
        ajuda = rand()%9;
        }
        jogada = ajuda;
        adiciona(tabuleiro,1+jogada,jogador);
            } 
            jogador=1;
            if(aux->possib[jogada]!=NULL)
                aux = aux->possib[jogada]; //segue a arvore junto com o jogo
            //}
        }

        if(checa1(tabuleiro))  //ver se o jogador X ganhou
        {
            system("cls");
            cout<<"jogador X venceu!!!";
            cout<<"\n\n"<<endl;
            desenha(tabuleiro);
            cria(tabuleiro);
            jogador = 1;
            aux->max++;
            minimax(&possib,false); //salva a partida pra ver se o caminho é bom mesmo ou nao
            aux = possib; //volta a arvore pro começo
            cout << "\nDiga qual a dificuldade voce quer jogar, 1 para facil, 2 para medio, 3 para impossivel"<<endl;
            cin >> dificuldade;
            cin.clear();
            cin.ignore();
            while(dificuldade<1||dificuldade>3){
            cout << "\n Resposta invalida, diga qual a dificuldade voce quer jogar, 1 para facil, 2 para medio, 3 para impossivel"<<endl;
            cin >> dificuldade;
            cin.clear();
            cin.ignore();
            }
            system("pause");
        }
        if(checa2(tabuleiro))  //ver se o jogador 0 ganhou
        {
            system("cls");
            cout<<"jogador O venceu!!!";
            cout<<"\n\n"<<endl;
            desenha(tabuleiro);
            cria(tabuleiro);
            jogador = 1;
            aux->max--;
            minimax(&possib,false);
            aux = possib;
            cout << "\nDiga qual a dificuldade voce quer jogar, 1 para facil, 2 para medio, 3 para impossivel"<<endl;
            cin >> dificuldade;
            cin.clear();
            cin.ignore();
            while(dificuldade<1||dificuldade>3){
            cout << "\n Resposta invalida, diga qual a dificuldade voce quer jogar, 1 para facil, 2 para medio, 3 para impossivel"<<endl;
            cin >> dificuldade;
            cin.clear();
            cin.ignore();
            }
            system("pause");
        }
        int filho=0;
        for(int i = 0; i<9; i++)
        {
            if(aux->possib[i]==NULL)
                filho++;
        }
        if(filho==9)  //olha se deu empate
        {
            system("cls");
            cout<<"Ninguem venceu!!!";
            cout<<"\n\n"<<endl;
            desenha(tabuleiro);
            cria(tabuleiro);
            jogador = 1;
            aux = possib;
            cout << "\nDiga qual a dificuldade voce quer jogar, 1 para facil, 2 para medio, 3 para impossivel"<<endl;
            cin >> dificuldade;
            cin.clear();
            cin.ignore();
            while(dificuldade<1||dificuldade>3){
            cout << "\n Resposta invalida, diga qual a dificuldade voce quer jogar, 1 para facil, 2 para medio, 3 para impossivel"<<endl;
            cin >> dificuldade;
            cin.clear();
            cin.ignore();
            }
            system("pause");
        }
    }
    return 0;
}
