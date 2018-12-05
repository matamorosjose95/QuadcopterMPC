# Proyecto de Control de Cuadcópteros con MPC
### Modelo dinámico de un cuadcóptero
El modelo dinámico utilizado considera las siguientes ecuaciones de movimiento para la posición:

![ec x](http://latex.codecogs.com/png.latex?%5Cinline%20%5Cdpi%7B100%7D%20%5Csmall%20%5Cddot%7Bx%7D%3D%5Cfrac%7BU%7D%7Bm%7D%28cos%28%5Cpsi%29sin%28%5Ctheta%29sin%28%5Cphi%29&plus;sin%28%5Cpsi%29sin%28%5Cphi%29%29)

![ec y](http://latex.codecogs.com/png.latex?%5Cinline%20%5Cdpi%7B100%7D%20%5Csmall%20%5Cddot%7By%7D%3D%5Cfrac%7BU%7D%7Bm%7D%28sin%28%5Cpsi%29sin%28%5Ctheta%29cos%28%5Cphi%29-cos%28%5Cpsi%29sin%28%5Cphi%29%29)

![ec_z](http://latex.codecogs.com/png.latex?%5Cinline%20%5Cdpi%7B100%7D%20%5Csmall%20%5Cddot%7Bz%7D%3D%5Cfrac%7BU%7D%7Bm%7Dcos%28%5Ctheta%29cos%28%5Cphi%29-g)

Y las siguientes ecuaciones de movimiento para los ángulos *Roll*, *Pitch* y *Yaw*, respectivamente:

![ec_phi](http://latex.codecogs.com/png.latex?%5Cinline%20%5Cdpi%7B100%7D%20%5Csmall%20%5Cddot%7B%5Cphi%7D%3D%5Cfrac%7BI_%7Byy%7D-I_%7Bzz%7D%7D%7BI_%7Bxx%7D%7D%5Cdot%7B%5Ctheta%7D%5Cdot%7B%5Cpsi%7D&plus;%5Cfrac%7B%5Ctau_x%7D%7BI_%7Bxx%7D%7D)

![ec_theta](http://latex.codecogs.com/png.latex?%5Cinline%20%5Cdpi%7B100%7D%20%5Csmall%20%5Cddot%7B%5Ctheta%7D%3D%5Cfrac%7BI_%7Bzz%7D-I_%7Bxx%7D%7D%7BI_%7Byy%7D%7D%5Cdot%7B%5Cphi%7D%5Cdot%7B%5Cpsi%7D&plus;%5Cfrac%7B%5Ctau_y%7D%7BI_%7Byy%7D%7D)

![ec_psi](http://latex.codecogs.com/png.latex?%5Cinline%20%5Cdpi%7B100%7D%20%5Csmall%20%5Cddot%7B%5Cpsi%7D%3D%5Cfrac%7BI_%7Bxx%7D-I_%7Byy%7D%7D%7BI_%7Bzz%7D%7D%5Cdot%7B%5Cphi%7D%5Cdot%7B%5Ctheta%7D&plus;%5Cfrac%7B%5Ctau_z%7D%7BI_%7Bzz%7D%7D)

En donde ![ec_tau_x](http://latex.codecogs.com/png.latex?%5Cinline%20%5Cdpi%7B100%7D%20%5Csmall%20%5Ctau_x), ![ec_tau_y](http://latex.codecogs.com/png.latex?%5Cinline%20%5Cdpi%7B100%7D%20%5Csmall%20%5Ctau_y), ![ec_tau_z](http://latex.codecogs.com/png.latex?%5Cinline%20%5Cdpi%7B100%7D%20%5Csmall%20%5Ctau_z) y ![ec_u](http://latex.codecogs.com/png.latex?%5Cinline%20%5Cdpi%7B100%7D%20%5Csmall%20U) son:

![ec_taux](http://latex.codecogs.com/png.latex?%5Cinline%20%5Cdpi%7B100%7D%20%5Csmall%20%5Ctau_x%3DbL%28%5COmega_4%5E%7B2%7D-%5COmega_2%5E%7B2%7D%29)

![ec_tauy](http://latex.codecogs.com/png.latex?%5Cinline%20%5Cdpi%7B100%7D%20%5Csmall%20%5Ctau_y%3DbL%28%5COmega_3%5E%7B2%7D-%5COmega_1%5E%7B2%7D%29)

![ec_tauz](http://latex.codecogs.com/png.latex?%5Cinline%20%5Cdpi%7B100%7D%20%5Csmall%20%5Ctau_z%3Dd%28%5COmega_1%5E%7B2%7D-%5COmega_2%5E%7B2%7D&plus;%5COmega_3%5E%7B2%7D-%5COmega_4%5E%7B2%7D%29)

![ec_U](http://latex.codecogs.com/png.latex?%5Cinline%20%5Cdpi%7B100%7D%20%5Csmall%20U%3Db%28%5COmega_1%5E%7B2%7D&plus;%5COmega_2%5E%7B2%7D&plus;%5COmega_3%5E%7B2%7D&plus;%5COmega_4%5E%7B2%7D%29)

### Modelo lineal en variables de estado
El modelo lineal en variables de estado considera como vector de estados a ![ec_X](http://latex.codecogs.com/png.latex?%5Cinline%20%5Cdpi%7B100%7D%20%5Csmall%20X%3D%5Bx%2C%20%5Cdot%7Bx%7D%2C%20y%2C%20%5Cdot%7By%7D%2C%20z%2C%20%5Cdot%7Bz%7D%2C%20%5Cphi%2C%20%5Cdot%7B%5Cphi%7D%2C%20%5Ctheta%2C%20%5Cdot%7B%5Ctheta%7D%2C%20%5Cpsi%2C%20%5Cdot%7B%5Cpsi%7D%5D) y el vector de entradas ![ec_UU](http://latex.codecogs.com/png.latex?%5Cinline%20%5Cdpi%7B100%7D%20%5Csmall%20u%3D%5B%5COmega_1%2C%20%5COmega_2%2C%20%5COmega_3%2C%20%5COmega_4%5D), el cual fue linealizado en torno al siguiente punto de operación: ![ec_barX](http://latex.codecogs.com/png.latex?%5Cinline%20%5Cdpi%7B100%7D%20%5Csmall%20%5Cbar%7BX%7D%3D%5B0%2C%200%2C%200%2C%200%2C%20%5Cbar%7Bz%7D%2C%200%2C%200%2C%200%2C%200%2C%200%2C%200%2C%200%5D), ![ec_barU](http://latex.codecogs.com/png.latex?%5Cinline%20%5Cdpi%7B100%7D%20%5Csmall%20%5Cbar%7BU%7D%3D%5B192.767%2C%20192.767%2C%20192.767%2C%20192.767%5D), valor en el cual el sistema se encuentra en un punto de equilibrio dado por la altura ![](http://latex.codecogs.com/png.latex?%5Cinline%20%5Cdpi%7B100%7D%20%5Csmall%20%5Cbar%7Bz%7D arbitraria. De las ecuaciones de movimiento se puede observar que no existe influencia de la posición en la dinámica del sistema.

### Control MPC con acceso a todos los estados

### Control MPC usando filtro de Kalman
