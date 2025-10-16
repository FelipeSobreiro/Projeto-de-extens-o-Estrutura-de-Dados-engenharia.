#include <iostream>
#include <vector>
#include <string>
#include <stdexcept>
#include <limits>
#include <algorithm>
#include <cctype>
using namespace std;

void Insert(string A, float B, char f);
void cor(string A, int b);
int cont(float A);
bool Remove(float A);

struct item {
    string Nome;
    float Ohms;
    char unidade;
};

vector<item> C;

int main() {
    int escolha;

    do {
        cout << "\n\t== PROGRAMA DE REESTOQUE DE RESISTORES ==\n";
        cout << "ESCOLHA UMA DAS OPCOES A SEGUIR:\n";
        cout << "(1) ADICIONAR NOVO RESISTOR\n";
        cout << "(2) REMOVER ITEM\n";
        cout << "(3) MOSTRAR IMAGEM DE RESISTORES \n";
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
            char unidade;

            cout << "Digite o valor do resistor: ";
            cin >> R;

            cout << "Escolha A Unidade Do Resistor : \n";
            cout << "(K) KILO\n";
            cout << "(M) MEGA\n";
            cout << "(G) GIGA\n";
            cout << "(U) UNIDADE DECIMAL\n";
            cin >> unidade;
            unidade = toupper(unidade);
            



            Insert(nome, R,unidade);
            break;
        }
        case 2: {
            float temp;
            cout << "Digite o valor do resistor que voce quer remover: ";
            cin >> temp;
            Remove(temp); 
            break;
        }
        case 5: {
            if (C.empty()) {
                cout << "Nenhum resistor cadastrado ainda!\n";
            }
            else {
                for (const auto& elemento : C) {
                    cout << "Nome: " << elemento.Nome
                        << " | Ohms: " << elemento.Ohms <<elemento.unidade;

                }
            }
            break;
        }
        case 6: {
            cout << "Total de resistores cadastrados: " << C.size() << endl;
            break;
        }
        case 7: {
            float temp1;
            cout << "Digite o valor do resistor: ";
            cin >> temp1;
            int a = cont(temp1);
            cout << "\tQuantidade de resistores em estoque: " << a << "\n";
            break;
        }
        case 8:
            cout << "Programa encerrado!\n";
            break;
        default:
            cout << "Opcao invalida. Tente novamente.\n";
        }
    } while (escolha != 8);

    return 0;
}

void Insert(string A, float B, char f) {
    try {
       
        if (f != 'K' && f != 'M' && f != 'G' && f != 'U') {
            f = 'U';
        }

        C.push_back({ A, B, f });
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

int cont(float A) {
    int contagem = 0;
    for (const auto& a : C) {
        if (a.Ohms == A) {
            contagem++;
        }
    }
    return contagem;
}

bool Remove(float A) {
    if (C.empty()) {
        cout << "Nenhum resistor cadastrado!\n";
        return false;
    }

  
    sort(C.begin(), C.end(), [](const item& x, const item& y) {
        return x.Ohms < y.Ohms;
        });

    int n = 0;
    int j = C.size() - 1;

    while (n <= j) {
        int meio = (n + j) / 2;

        if (C[meio].Ohms == A) {
            C.erase(C.begin() + meio);
            cout << "Resistor removido com sucesso!\n";
            return true;
        }
        else if (C[meio].Ohms > A) {
            j = meio - 1;
        }
        else {
            n = meio + 1;
        }
    }

    cout << "Resistor nao encontrado!\n";
    return false;
}

void graf() {
    cout << "\tGRAFICO DOS RESISTORES\n";
    for (auto const a : C) {

        cout << a.Ohms;

    }
}
