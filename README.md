# Projeto-de-extens-o-Estrutura-de-Dados-engenharia.
projeto de extensão da matéria estrutura de dados, faculdade de engenharia controle e automação e elétrica. 
#include <iostream>
#include <vector>
#include <string>
#include <stdexcept>
#include <limits> 
using namespace std;


void Insert(string A, float B);
void cor(string A, int b);
int cont(int A);


struct item {
    string Nome;
    float Ohms;
};


vector<item> C;

int main() {
    int escolha;

    do {
        cout << "\n\t== PROGRAMA DE REESTOQUE DE RESISTORES ==\n";
        cout << "ESCOLHA UMA DAS OPCOES A SEGUIR:\n";
        cout << "(1) ADICIONAR NOVO RESISTOR\n";
        cout << "(2) REMOVER ITEM\n";
        cout << "(3) MOSTRAR IMAGEM DE RESISTORES DISPONIVEIS\n";
        cout << "(4) MOSTRAR GRAFICO DE ESTOQUE\n";
        cout << "(5) MOSTRAR LISTA DE ITENS\n";
        cout << "(6) MOSTRAR TAMANHO DA LISTA\n";
        cout << "(7) MOSTRAR QUANT DE ITEM ESPECIFICO\n";
        cout << "(8) SAIR DO PROGRAMA\n";
        cout << "\n> ";

        cin >> escolha;
        cin.clear(); 
        cin.ignore(numeric_limits<streamsize>::max(), '\n'); 

        switch (escolha) {
        case 1: {
            string nome = "Resistor";
            float R;
            cout << "Digite o valor do resistor: ";
            cin >> R;
            cin.clear();
            cin.ignore(numeric_limits<streamsize>::max(), '\n');
            Insert(nome, R);
       
     
            break;
        }
        case 2: {
            int temp;
            cout << "Digite o valor do resistor que voce quer remover :";
            cin >> temp;


            break;
        }
        case 5: {
            if (C.empty()) {
                cout << "Nenhum resistor cadastrado ainda!\n";
            }
            else {
                for (const auto& elemento : C) {
                    cout << "Nome: " << elemento.Nome
                        << " | Ohms: " << elemento.Ohms << endl;
                }
            }
            break;
        }
        case 6: {
            cout << "Total de resistores cadastrados: " << C.size() << endl;
            break;
        }

        case 7: {
            int temp1;
            cout << "DIGITE O VALOR DO RESISTOR\n";
            cin >> temp1;
           int a= cont(temp1);

           cout << "\tQUANTIDADE DE RESISTORES EM ESTOQUE : " << a << "\n";

            break;
        }


        case 8:
            cout << "PROGRAMA ENCERRADO!\n";
            break;
        default:
            cout << "Opcao invalida. Tente novamente.\n";
        }

    } while (escolha != 8);

    return 0;
}


void Insert(string A, float B) {
    try {
        C.push_back({ A, B });
        cout << "Resistor adicionado!\n";

    }
    catch (const exception& e) {
        cout << "Erro ao adicionar o resistor: " << e.what() << endl;
    }
}


void cor(string A, int b) {
    switch (b) {
    case 1: cout << "\033[31m" << A << "\033[0m\n"; break;
    case 2: cout << "\033[32m" << A << "\033[0m\n"; break;
    case 3: cout << "\033[33m" << A << "\033[0m\n"; break;
    case 4: cout << "\033[34m" << A << "\033[0m\n"; break;
    default: cout << A << "\n"; break;
    }
}

int cont(int A) {
    int contagem = 0;
    for (const auto& a : C) {
        if (a.Ohms == A) {
            contagem += 1;
        }
    }
    return contagem;
}


