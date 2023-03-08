FROM jupyter/minimal-notebook:ubuntu-20.04

USER root
RUN apt-get update && apt-get install -y libmysqlclient-dev gcc
# RUN python3 -m pip install 'ideas-uo @ git+https://github.com/HPCL/ideas-uo@5f9adfc0dd80038460f1d14a6518e65cd7dd63de'
RUN python3 -m pip install 'ideas-uo @ git+https://github.com/frobnitzem/ideas-uo@ae3dc4ef3b08fac3d9940e814d0ebaf605b6474f'

# Return to original notebook user
USER jovyan
#CMD /bin/bash
