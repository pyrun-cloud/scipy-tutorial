# Start from a base Python image
FROM public.ecr.aws/docker/library/python:3.10-slim-buster

# apt installations
RUN apt update && \
    apt install -y --no-install-recommends \
        gcc \
        git \
        build-essential \
        cmake \
        zlib1g-dev \ 
        pdal \ 
        libpdal-dev

# LAStools for LiDAR files
RUN  git clone https://github.com/aitorarjona/LAStools.git -b lasquery && \
     cd LAStools && \
     mkdir build && \
     cd build && \
     cmake .. && \
     make && \
     cp ../bin64/lasindex64 /bin/lasindex && \
     cp ../bin64/lasquery64 /bin/laxquery && \
     cd /

# gztool for fastqgzip files
RUN git clone https://github.com/circulosmeos/gztool.git && \
    cd gztool && \
    gcc -O3 -o gztool gztool.c -lz -lm && \
    cp gztool /bin/gztool && \
    cd /

# Set up the function directory (standard for some cloud function environments)
ARG FUNCTION_DIR="/function"
RUN mkdir -p ${FUNCTION_DIR}
WORKDIR ${FUNCTION_DIR}

# Copy requirements (optional, can install directly)
# COPY requirements.txt .

# Install Python packages using pip
# Ensure LITHOPS integration variable is available if needed
ARG LITHOPS
RUN pip install --no-cache-dir \
    pandas \
    numpy \
    requests \
    "$LITHOPS" \
    git+https://github.com/CLOUDLAB-URV/dataplug \
    cloud-data-cockpit \
    pyimzml \ 
    rasterio \
    ipykernel \
    ipywidgets \
    awscli \ 
    awslambdaric \
    pandas \
    seaborn \
    matplotlib \ 
    redis \ 
    ray \
    scikit-learn \ 
    laspy \ 
    pdal==2.3.0 \
    lazrs \ 
    pyproj
    # Add other pip packages here or install from requirements.txt
    # If using requirements.txt: RUN pip install --no-cache-dir -r requirements.txt

# Ensure Python output is not buffered
ENV PYTHONUNBUFFERED=TRUE

# (Optional) Define entrypoint or command if needed
# CMD ["python", "your_script.py"] 