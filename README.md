#include <bits/stdc++.h>

using namespace std;

double simplex(vector<vector<double>>& tabla, int M, int N) {
       int columna = 0;
       for (int j = 1; j < N; j++) { 
           if (tabla[M-1][j] < tabla[M-1][columna]) { 
               columna = j; 
           }
       }
       if (tabla[M-1][columna] >= 0) {  //sacra el valor optimo
           return 0; 
       }
       int fila = -1; 
       for (int i = 0; i < M-1; i++) { 
           if (tabla[i][columna] > 0 && (fila == -1 || tabla[i][N-1]/tabla[i][columna] < tabla[fila][N-1]/tabla[fila][columna])) {
               fila = i; 
           }
       }
       if (fila == -1) { //no c encontro ningunaa fila valida
           return 0; 
       }
       double pivote = tabla[fila][columna];
       for (int j = 0; j < N; j++) { 
           tabla[fila][j] /= pivote; 
       }
       for (int i = 0; i < M; i++) { 
           if (i != fila) {
               double elementoPivote = tabla[i][columna]; 
               for (int j = 0; j < N; j++) {
                   tabla[i][j] -= elementoPivote * tabla[fila][j]; 
               }
           }
       }
   return simplex(tabla, M, N);
}


int main(){
    cin.tie(nullptr);
    ios_base::sync_with_stdio(false);

    int N, M;
    string restricciones;

    cin >> N >> M;

    vector<vector<double>> tabla(M+1, vector<double>(N+M+2));

    for (int i = 0; i < M; i++) { 
       for (int j = 0; j < N; j++) {
           cin >> tabla[i][j];
       }
       cin >> restricciones; 
       cin >> tabla[i][N+M]; 
       tabla[i][N+i] = 1; //variable de holgura en el coef restric actual como 1 
       if (restricciones != "<=" && ((tabla[i][N+M] *= -1) >= 0)) {
         for (int j = 0; j < N; j++){
           tabla[i][j] *= -1;
         }
       }
       tabla[M][N+M] = 1; //variable de holgura en el coef fun obejct como 1
   }

   for (int j = 0; j < N; j++) { 
       cin >> tabla[M][j]; 
       tabla[M][j] *= -1; 
   }

    tabla[M][N+M] = 0; //variable de holgura en el coef fun object como 0
   cout.precision(14); 
   simplex(tabla, M+1, N+M+1); 
   for (int i = 0; i < M; i++) { 
       if (tabla[i][N+M] < 0) { 
           cout << "No se satisfacen las restricciones" << "\n";
           return 0; 
       }
   }

   for (int i = 0; i < N; i++) { 
        bool basica = true; 
       int fila = -1; 
        for (int j = 0; j < M; j++) { 
           if (abs(tabla[j][i]) > 0) {
               if (fila == -1) { 
                   fila = j;
               } else {
                   basica = false; 
                   break;
               }
           }
       }
       if (basica) {
           cout<< tabla[fila][N+M] << "\n";
       } else {
           cout << 0 <<"\n";
       }
   }
   cout << tabla[M][N+M] << "\n";

    return 0;
}
