# algorithmes de trie + complexité

import time
import random
import matplotlib.pyplot as plt
import numpy as np

#tableau de 100 elements:
T1=[random.randint(1,100) for _ in range(100)]


#Tableau de 1000 elements:
T2=[random.randint(1,100) for _ in range(1000)]


#tableau de 10000 elements:
T3=[random.randint(1,100) for _ in range(10000)]



#tri par insertion:
def tri_insertion(T):
    for i in range(1, len(T)):
        k = T[i]
        j = i - 1
        while j >= 0 and k < T[j]:
            T[j + 1] = T[j]
            j -= 1
        T[j + 1] = k
    return T

#tri par selection:
def tri_selection(T):
    n = len(T)
    for i in range(n):
        min_index = i
        for j in range(i + 1, n):
            if T[j] < T[min_index]:
                min_index = j
        T[i], T[min_index] = T[min_index], T[i]
    return T

# tri a bulle:
def tri_bulle(T):
    n = len(T)
    for i in range(n):
        for j in range(0, n - i - 1):
            if T[j] > T[j + 1]:
                T[j], T[j + 1] = T[j + 1], T[j]
    return T


# tri par fusion:
def tri_fusion(T):
    if len(T) > 1:
        mid = len(T) // 2
        L = T[:mid]
        R = T[mid:]

        tri_fusion(L)
        tri_fusion(R)

        i = j = k = 0

        while i < len(L) and j < len(R):
            if L[i] < R[j]:
                T[k] = L[i]
                i += 1
            else:
                T[k] = R[j]
                j += 1
            k += 1

        while i < len(L):
            T[k] = L[i]
            i += 1
            k += 1

        while j < len(R):
            T[k] = R[j]
            j += 1
            k += 1
    return T

#tri par tas:
def tri_tas(T):
    def heapify(T, n, i):
        largest = i
        left = 2 * i + 1
        right = 2 * i + 2

        if left < n and T[left] > T[largest]:
            largest = left

        if right < n and T[right] > T[largest]:
            largest = right

        if largest != i:
            T[i], T[largest] = T[largest], T[i]
            heapify(T, n, largest)

    n = len(T)
    for i in range(n // 2 - 1, -1, -1):
        heapify(T, n, i)

    for i in range(n - 1, 0, -1):
        T[i], T[0] = T[0], T[i]
        heapify(T, i, 0)
    return T

#tri par dichotomie:
def tri_dichotomie(T):
    if len(T) <= 1:
        return T
    else:
        pivot = T[len(T) // 2]
        left = [x for x in T if x < pivot]
        middle = [x for x in T if x == pivot]
        right = [x for x in T if x > pivot]
        return tri_dichotomie(left) + middle + tri_dichotomie(right)


#mesurer le temps d'execution de chaque algorithme de tri sur chaque tableau 10 fois:(retourne moyenne)
def mesurer_temps(tri_fonction, T):
    total_time = 0
    for _ in range(10):
        T_copy = T.copy()
        start_time = time.time()
        tri_fonction(T_copy)
        end_time = time.time()
        total_time += (end_time - start_time)
    return total_time / 10

#test des differentes methodes de tri sur les trois tableaux:
algorithmes_de_tri = [tri_insertion, tri_selection, tri_bulle, tri_fusion, tri_tas, tri_dichotomie]
for algorithme in algorithmes_de_tri:
    print(f"Temps moyen pour {algorithme.__name__} sur T1 (100 elements): {mesurer_temps(algorithme, T1)} secondes")
    print(f"Temps moyen pour {algorithme.__name__} sur T2 (1000 elements): {mesurer_temps(algorithme, T2)} secondes")
    print(f"Temps moyen pour {algorithme.__name__} sur T3 (10000 elements): {mesurer_temps(algorithme, T3)} secondes")
    print("--------------------------------------------------")



tailles = [100, 1000, 10000]
tableaux = [T1, T2, T3]

# Nom des algorithmes et leurs fonctions
algorithmes_de_tri = {
    "Insertion": tri_insertion,
    "Sélection": tri_selection,
    "Bulle": tri_bulle,
    "Fusion": tri_fusion,
    "Tas": tri_tas,
    "Dichotomique": tri_dichotomie
}

# Mesurer les temps une seule fois et stocker
resultats = {}
for nom, fonction in algorithmes_de_tri.items():
    resultats[nom] = {}
    for taille, T in zip(tailles, tableaux):
        resultats[nom][taille] = mesurer_temps(fonction, T)

# Complexités théoriques et coefficients pour l'échelle
complexites = {
    "Insertion": (lambda n: n**2, 1e7),
    "Sélection": (lambda n: n**2, 1e7),
    "Bulle": (lambda n: n**2, 1e7),
    "Fusion": (lambda n: n * np.log(n), 1e4),
    "Tas": (lambda n: n * np.log(n), 1e4),
    "Dichotomique": (lambda n: n * np.log(n), 1e4)
}

# 6. le temps d’exécution moyen T / x:
for nom, temps in resultats.items():
    N = np.array(list(temps.keys()))
    T = np.array(list(temps.values()))


    # --- Calcul des 4 rapports ---
    T_div_N = T / N
    T_div_logN = T / np.log2(N)
    T_div_NlogN = T / (N * np.log2(N))
    T_div_N2 = T / (N ** 2)

    # --- Normalisation pour rendre les courbes proches ---
    T_div_N /= np.max(T_div_N)
    T_div_logN /= np.max(T_div_logN)
    T_div_NlogN /= np.max(T_div_NlogN)
    T_div_N2 /= np.max(T_div_N2)

    # --- Graphe comparatif avec échelle normalisée ---
    plt.figure(figsize=(8, 5))
    plt.plot(N, T_div_N, marker='o', label="T / N")
    plt.plot(N, T_div_logN, marker='s', label="T / log N")
    plt.plot(N, T_div_NlogN, marker='^', label="T / (N log N)")
    plt.plot(N, T_div_N2, marker='d', label="T / N²")
    plt.title(f"verification de la complexite - {nom}")
    plt.xlabel("Taille du tableau (N)")
    plt.ylabel("Valeur normalisée (0–1)")
    plt.legend()
    plt.grid(True)
    plt.show()
