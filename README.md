# [PAX7STAF - Variabilité Climatique et Environnementale](https://chamilo.univ-grenoble-alpes.fr/courses/PAX7STAF/index.php?id_session=0)

Mickaël LALANDE (mickael.lalande@univ-grenoble-alpes.fr) - Octobre 2020

Pour télécharger ce dossier, cliquez sur le bouton vert `Code` et vous pouvez tout télécharger directement en zip.

## Projets tutorés :
- [Biais froid sur le Plateau Tibétain et projections climatiques (CMIP6)](HMA_bias_CMIP6)

  
## Installation de l'environnement de travail :

1. **Installer [Miniconda](https://docs.conda.io/en/latest/miniconda.html)** : 

Miniconda est plus léger et permet de n'installer que les paquets dont vous avez besoin, mais si vous avez déjà une intallation Anaconda ça fera l'affaire (passez au point 2.) ! Il faut par contre une version **Python 3**, donc si vous êtes sur Python 2, réinstallez une version Python 3.

- **Windows/Mac** :

Télécharger la version de Miniconda correspondant à votre machine sur : https://docs.conda.io/en/latest/miniconda.html

- **Linux** (dans un terminal):

```bash
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh 
sh Miniconda3-latest-Linux-x86_64.sh 
  
# This will automatically update your ~/.bashrc so that you can directly have
# conda in your path
Do you wish the installer to initialize Miniconda3
by running conda init? [yes|no]
[no] >>> yes
  
source ~/.bashrc  
```
  
Vous devriez avoir `(base)` devant votre ligne dans le terminal, qui correspond à l'environnement **root**.


2. **Installer l'environnement de travail**

Pour Windows et Mac, essayez de trouver l'invite de commande de Miniconda, autrement si vous n'avez que l'installation graphique, allez dans les environnements et essayez d'importer le fichier [phd_v3_fh.yml](envs/phd_v3_fh.yml) (dans le dossier [envs](envs)) pour créer le nouvel environnement. Autrement essayez d'installer les packages indiquées ci-dessous dans la commande `conda install ...`

Pour ceux qui réussisent à ouvrir un terminal dans lequel la commande `conda` est disponible, vous pouvez essayer les commandes suivantes :

```bash
conda config --add channels conda-forge  
conda config --set channel_priority strict  
conda update -n base -c defaults conda  
```

- **Installation automatique** (normalement ok pour **Mac** et **Linux**) :

Le fichier `phd_v3_fh.yml` est dans le dossier [envs](envs).

```bash
conda env create -f phd_v3_fh.yml 
```

- **Installation manuelle** (pour **Windows**)

Pour Windows le package `xesmf` n'est pas disponible, il n'est donc pas possible de l'utiliser pour faire les interpolations. Une alternative sera donc utilisées (voir: http://xarray.pydata.org/en/stable/interpolation.html).

```bash
conda create -n work
conda activate work

conda install xarray dask nb_conda_kernels jupyter jupyterlab \
"nodejs>=10.0" netcdf4 proplot cartopy "matplotlib<=3.2" intake-esm \
python-graphviz nbresuse nc-time-axis gcsfs zarr nbresuse netcdf4
  
jupyter lab
```

- **Installation manuelle** (pour **Linux/Mac** si l'installation ci-dessus n'a pas fonctionnée) :

```bash
conda create -n work
conda activate work

conda install xarray xesmf esmpy dask nb_conda_kernels jupyter jupyterlab \
"nodejs>=10.0" netcdf4 proplot cartopy "matplotlib<=3.2" intake-esm \
python-graphviz nbresuse nc-time-axis gcsfs zarr nbresuse netcdf4

# Fot testing xESMF
pip install pytest  
pytest -v --pyargs xesmf  #all need to pass
  
# Launch jupyter lab
jupyter lab
```

Il faudra que vous pensiez à faire ensuite à chaque fois lorsque vous lancer l'invite de commande :
```bash
conda activate work
jupyter lab
```

## Information sur les packages


- **[xesmf](https://xesmf.readthedocs.io/en/latest/)** / esmpy (pour faire des regrids, **pas disponible sous Windows**)
- **[jupyter](https://jupyter.org/)** (pour les Jupyter Notebook)
- **[xarray](http://xarray.pydata.org/en/stable/)** / netcdf4 / nc-time-axis (pour lire et manipuler les fichiers netCDF)
- **[proplot](https://proplot.readthedocs.io/en/latest/)** / [cartopy](https://scitools.org.uk/cartopy/docs/latest/) / "[matplotlib](https://matplotlib.org/)<=3.2" (pour faire des cartes)
- **[intake-esm](https://intake-esm.readthedocs.io/en/latest/)** / gcsfs / zarr (pour accéder aux données CMIP6)

Optionnels :

- **[dask](https://dask.org/)** / python-graphviz  (pour paralléliser)
- **nb_conda_kernels** (pour pouvoir changer de noyau/d'environnement à l'interieur d'un Notebook)
- **jupyterlab** / "nodejs>=10.0" (plus sympa que Jupyter Notebook)
- **nbresuse** (pour voir la RAM utilisée dans un Notebook)

## Conseils et astuces

Informations sur les environnements : https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html


Il est recommandé de ne pas utiliser l'environnement **root** (base) afin de garder une installation propre (voir cette [note](https://conda-forge.org/docs/user/introduction.html) en fin de page). 

[Linux] Vous pouvez ajouter `conda activate my_env` dans votre `.bashrc` afin de ne pas avoir à l'activer à chaque fois.

Une fois que vous avez mis en place un environnement, je vous conseille de le sauvegarder (`conda list --explicit > spec-file.txt `) avant de faire des mises à jour... parfois vous pouvez tout casser. Une autre option est de faire des versions de votre environnement, de sorte qu'avant une mise à jour ou une installation d'un nouveau paquet, vous puissiez cloner votre environnement (`conda create --name myenv_v1 --clone myenv_v0`) avec un nouveau numéro de version par exemple (voir https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html pour plus d'infos sur les environnements). Une bonne pratique consiste alors à noter la version de votre environnement que vous utilisez dans vos scripts afin d'être sûr de le conserver pour fonctionner encore plus tard.
  
