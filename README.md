#include <iostream>
#include <vector>
#include <string>
#include <stdexcept>
#include <limits>
#include <algorithm>
#include <cctype>
#include <cmath>
#include <map>
using namespace std;


struct item {
    string Nome;
    float Ohms;
    char unidade;
    int quant;
};

vector<item> C;

void Insert(string A, float B, char f, int quant);
int cont(float A);
bool Remove(float A);
void graf();
float Calcular_resistencia(string a, string b, string c, string tole);


string toLower(string s) {
    transform(s.begin(), s.end(), s.begin(), [](unsigned char c) { return tolower(c); });
    return s;
}

int main() {
    int escolha;

    do {
        cout << "\t=====================================================\n";
      
        cout << "\t== PROGRAMA DE REESTOQUE DE RESISTORES ==\n";
        cout << "\t=====================================================\n";
      
        cout << "(1) ADICIONAR NOVO RESISTOR\n";
        cout << "(2) REMOVER ITEM\n";
    
        cout << "(3) MOSTRAR GRAFICO DE ESTOQUE\n";
        cout << "(6) MOSTRAR TAMANHO DA LISTA\n";
     
        cout << "(7) MOSTRAR QUANTIDADE DE ITEM ESPECIFICO\n";
     
        cout << "(8) CALCULAR RESISTENCIA COM BASE NAS CORES\n";
        cout << "(9) SAIR DO PROGRAMA\n";
        cout << "\n> ";

        cin >> escolha;
        cin.clear();
        cin.ignore(numeric_limits<streamsize>::max(), '\n');

        switch (escolha) {
        case 1: {
            string nome = "Resistor";
            float R;
            char unidade;
            int quant;

            cout << "Digite o valor do resistor: ";
          
            if (!(cin >> R)) {
                cout << "Entrada invalida. Tente novamente.\n";
                cin.clear();
                cin.ignore(numeric_limits<streamsize>::max(), '\n');
                break;
            }

            cout << "Escolha a unidade do resistor: \n";
           
            cout << "(K) KILO\n(M) MEGA\n(G) GIGA\n(U) UNIDADE DECIMAL\n> ";
            if (!(cin >> unidade)) {
                unidade = 'U'; 
            }
            cin.clear();
            cin.ignore(numeric_limits<streamsize>::max(), '\n');
            unidade = toupper(unidade);

            cout << "Digite a quantidade: ";
            if (!(cin >> quant)) {
                cout << "Entrada invalida. Tente novamente.\n";
                cin.clear();
                cin.ignore(numeric_limits<streamsize>::max(), '\n');
                break;
            }
            cin.clear();
            cin.ignore(numeric_limits<streamsize>::max(), '\n');


            Insert(nome, R, unidade, quant);
            break;
        }
        case 2: {
            float temp;
            cout << "Digite o valor do resistor que deseja remover: ";
            if (!(cin >> temp)) {
                cout << "Entrada invalida. Tente novamente.\n";
                cin.clear();
                cin.ignore(numeric_limits<streamsize>::max(), '\n');
                break;
            }
            Remove(temp);
            break;
        }
        case 3: {
            graf();
            break;
        }
        case 6: {
            cout << "Total de resistores cadastrados: " << C.size() << endl;
            break;
        }
        case 7: {
            float temp1;
            cout << "Digite o valor do resistor: ";
            if (!(cin >> temp1)) {
                cout << "Entrada invalida. Tente novamente.\n";
                cin.clear();
                cin.ignore(numeric_limits<streamsize>::max(), '\n');
                break;
            }
            int a = cont(temp1);
            
            cout << "\tQuantidade em estoque: " << a << "\n";
            break;
        }
        case 8: {
            cout << "\nExemplo visual (resistor de 4 faixas):\n";
            cout << "┌───────────────┐\n";
            cout << "│ --marrom--preto--vermelho--dourado-- │\n";
           
            cout << "(Exemplo: marrom, preto, vermelho, dourado = 1k Ohm +/- 5%)\n\n";

            string a, b, c, d;
            cout << "Digite a 1a cor: ";
            cin >> a;
            cout << "Digite a 2a cor: ";
            cin >> b;
            cout << "Digite a 3a cor (multiplicador): ";
            cin >> c;
           
            cout << "Digite a 4a cor (tolerancia): ";
            cin >> d;

            float r = Calcular_resistencia(a, b, c, d);
            if (r > 0) {
             
                cout << "Resistencia calculada com sucesso!\n";
            }
            break;
        }
        case 9:
            cout << "Encerrando o programa...\n";
            break;
        default:
            
            cout << "Opcao invalida. Tente novamente.\n";
        }
    } while (escolha != 9);

    return 0;
}



void Insert(string A, float B, char f, int quant) {
    try {
        if (f != 'K' && f != 'M' && f != 'G' && f != 'U') {
            f = 'U';
        }
        C.push_back({ A, B, f, quant });
        
        cout << "Resistor adicionado com sucesso!\n";
    }
    catch (const exception& e) {
        cout << "Erro ao adicionar resistor: " << e.what() << endl;
    }
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
            
            cout << "Removendo: " << C[meio].Nome << " - " << C[meio].Ohms << " Ohm\n";
            C.erase(C.begin() + meio);
            
            cout << "Resistor removido com sucesso!\n";
            return true;
        }
        else if (C[meio].Ohms > A) j = meio - 1;
        else n = meio + 1;
    }

    
    cout << "Resistor nao encontrado!\n";
    return false;
}

int cont(float A) {
    int contagem = 0;
    for (const auto& a : C) {
        if (a.Ohms == A) contagem += a.quant;
    }
    return contagem;
}

void graf() {
    
    if (C.empty()) {
        cout << "\nNenhum resistor cadastrado!\n";
        return;
    }

    
    cout << "\n\t=== GRAFICO DE ESTOQUE ===\n";
    for (const auto& a : C) {
        
        cout << a.Nome << " - " << a.Ohms << a.unidade << " Ohm: ";
        for (int i = 0; i < a.quant; i++) cout << "█";
        cout << " (" << a.quant << ")\n";
    }
    cout << "===========================\n";
}


float Calcular_resistencia(string a, string b, string c, string tole) {
    map<string, int> cores{
        {"preto", 0}, {"marrom", 1}, {"vermelho", 2}, {"laranja", 3},
        {"amarelo", 4}, {"verde", 5}, {"azul", 6}, {"violeta", 7},
        {"cinza", 8}, {"branco", 9}
    };

    map<string, float> operacao = {
        {"preto", 1}, {"marrom", 10}, {"vermelho", 100}, {"laranja", 1000},
        {"amarelo", 10000}, {"verde", 100000}, {"azul", 1000000},
        {"violeta", 10000000}, {"cinza", 100000000}, {"branco", 1000000000},
        {"dourado", 0.1}, {"prateado", 0.01}
    };

    
    map<string, string> tol = {
        {"marrom", "+/-1%"}, {"vermelho", "+/-2%"}, {"verde", "+/-0.5%"},
        {"azul", "+/-0.25%"}, {"violeta", "+/-0.1%"}, {"cinza", "+/-0.05%"},
        {"dourado", "+/-5%"}, {"prateado", "+/-10%"}
    };


    a = toLower(a);
    b = toLower(b);
    c = toLower(c);
    tole = toLower(tole);

    if (cores.count(a) && cores.count(b) && operacao.count(c) && tol.count(tole)) {
        float resistencia = (cores[a] * 10 + cores[b]) * operacao[c];
      
        cout << "\nResistencia calculada: " << resistencia << " Ohm " << tol[tole] << endl;
        return resistencia;
    }
    else {
        
        cout << "COR DIGITADA ERRADA! Verifique a ortografia.\n";
        return 0;
    }
}
