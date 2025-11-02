**( Un pdf nommé : Getting\_Started2 : contient les questions 1 et 2 de l'ex 1 ***

Voir le pdf pour les équations
1-  Dériver les équations de mouvements pour les 2 masses:
2_Réduire ce système des équations
 voir le pdf pour la réponse:

3_ Une fonction equations_motion()
def equations_motion(t, y, m, k, kp):
    x1, v1, x2, v2 = y
    dx1dt = v1
    dv1dt = (-k * x1 + kp * (x2 - x1)) / m
    dx2dt = v2
    dv2dt = (-k * x2 + kp * (x1 - x2)) / m
    return [dx1dt, dv1dt, dx2dt, dv2dt]


4_ L'aide de la fonction **scipy.integrate.solve_ivp()
a)** Le code utilise scipy.integrate.solve_ivp pour intégrer ces équations du mouvement sur une durée donnée (ici 60 s) avec les  conditions initiales données dans l'énoncé :

b) on va résoudre ce système d'équations diff avec la méthode de Runge_kutta du 4ème ordre.
"" Cette Partie est un extrait de mon code""
k = 4.0
kp = 0.5
sol = solve_ivp(equations_motion, t_span, y0, t_eval=t_eval,
                args=(m, k, kp), method='RK45', rtol=1e-8, atol=1e-8)
if not sol.success:
    raise RuntimeError("Erreur de convergence ")

plt.figure(figsize=(8,4))
plt.plot(sol.t, sol.y[0], label="x1(t)", color="tab:blue")
plt.plot(sol.t, sol.y[2], label="x2(t)", color="tab:orange", linestyle="--")
plt.xlabel("Temps [s]")
plt.ylabel("Position [m]")
plt.title("Oscillateurs harmoniques couplés k=4, kp=0.5")
plt.legend()
plt.grid(True)
plt.tight_layout()
plt.show()

5_ Calcul la transformée de Fourier des résultats obtenus :
fft_x1 = np.fft.fft(sol.y[0])
fft_x2 = np.fft.fft(sol.y[2])

puis tu traces le spectre d’amplitude :
∣FFT(x1​)∣,∣FFT(x2​)∣
Cela fait apparaître deux pics de fréquence, correspondant aux deux modes propres du système

Résultat numérique (extrait)

Fréquences théoriques (Hz) : f1 = 0.318310, f2 = 0.355881

Fréquences numériques dominantes trouvées par FFT (pour k=4) : environ [0.316614, 0.349942]

Les valeurs numériques sont très proches des valeurs analytiques (petit décalage dû à la résolution en fréquence finie de la FFT et aux effets numériques).

Comparaison à l'oscillateur harmonique simple

. Ici, le mode en phase a exactement cette fréquence — physiquement, quand les deux masses oscillent en phase, le ressort de couplage ne se déforme (pas d'extension nette), donc il n'influence pas la fréquence de ce mode ; le mode en opposition voit le ressort de couplage se comprimer/étirer et sa raideur ajoute donc au total


6_ les étapes 4) et 5) avec différentes constantes k et k' mais meme conditions initiales.

En plus, vous pouvez voir un graphe représentant les 2 modes dominants en fonction de k/k'.

( identifications des régimes d'oscillation différentes)
( la répresentation graphique sera affichée lors de la compilation du code)

Voir lors de la compilation pour la représentation graphique:


Exercice2_

1-Résoudre numériquement en faisant varier 𝛼 et 𝛽.
Tracer les trajectoires et commenter les effets de la friction.

# Fonction représentant le mouvement

def equations(t, y, m, g, alpha, beta):
    x, y_pos, vx, vy = y
    v = np.sqrt(vx**2 + vy**2)  # vitesse totale
    ax = - alpha * vx - beta * vx * v / m # accélération selon x
    ay = - g - alpha * vy - beta * vy * v / m # accélération selon y ( Vous trouverez dans mon pdf l'explication mathématique)
    return [vx, vy, ax, ay] # retourne la dérivée du vecteur 


et aprés je vais traiter 3 cas différents : 


cas 1 : alpha=0 et beta=0 (sans frottement) --> Portée = 40.91 m
Cas 2 : alpha=0 et beta=0.05 (frottement quadratique seul) --> Portée = 17.57 m
Cas 3 : alpha=0.1 et beta=0 (frottement linéaire seul) --> Portée = 34.14 m



2-Étudier la portée du projectile selon l’angle  et discussions : 


for phi_deg in angles_deg:  # le but de cette fonction est de faire la conversion en radians,j'ai essayé de l'utiliser cette fonction, dans le but d'etre plus dans le contexte de la mécanique

  phi = np.radians(phi_deg)
    y0 = [0.0, 0.0, v0*np.cos(phi), v0*np.sin(phi)]

    sol = solve_ivp(
        fun=equations,
        t_span=(t0, tf),
        y0=y0,
        t_eval=t_eval,
        args=(m, g, alpha_study, beta_study),
        rtol=1e-8,
        atol=1e-8
    ) 


Voir les graphes : 
Discussion : Le frottement réduit la portée et déplace l’angle optimal de lancement en dessous de 45°. Les angles trop faibles ou trop élevés donnent une portée moindre.

Si α = β = 0 → projectile suit une parabole parfaite (physique idéale).

Si α ou β > 0 → la portée diminue, la hauteur maximale aussi, et la trajectoire devient plus « aplatie ».

β (quadratique) devient très important à grandes vitesses, α (linéaire) agit proportionnellement à la vitesse, donc plus constant sur la durée.
# # # # # # # # # # # # # # # # # # # # # # # # # # # # 


Bibliothèques utilisées : numpy, matplotlib, scipy.integrate
Ce travail ( Ex2)  a pour objectif de modéliser et de simuler le mouvement d’un projectile
soumis à la gravité et à des forces de frottement de type linéaire et/ou quadratique.

Le programme résout numériquement les équations différentielles du mouvement
à l’aide du solveur `solve\_ivp` de SciPy.

Il permet :
 - de tracer les trajectoires du projectile pour différents modèles de frottement ;
 - de calculer et comparer les portées atteintes ;
 - d’étudier la dépendance de la portée en fonction de l’angle de lancement.



