# Action_anticipation
Aim of our project, First Extract the features,and follow their optical flow with help of video frame gradients in X,Y axes. Our final aim is to use optical flow features to also predict regularities and irregularities in a video.
Dependencies:

    OpenCV version 2.4 or greater (dev version or install from source)
    PCL 1.6
    Boost version 1.50 or greater

Commands to install the required dependencies and run anticipation code on Subject 1 data:

# install pcl
sudo add-apt-repository ppa:v-launchpad-jochen-sprickerhof-de/pcl 
sudo apt-get update
sudo apt-get install libpcl-all

# install opencv
sudo apt-get install libopencv-dev
#install boost 1.50
wget http://sourceforge.net/projects/boost/files/boost/1.50.0/boost_1_50_0.tar.bz2
tar --bzip2 -xf boost_1_50_0.tar.bz2
#If you prefer to install boost to a specific directory use the following instead
# ./bootstrap.sh --prefix=path/to/installation/prefix
./bootstrap.sh
./b2
sudo ./b2 install

# download code 
git clone https://github.com/hemakoppula/human_activity_anticipation.git

# compile
cd human_activity_anticipation/build
cmake ..
make
cd ../src/pyobjs
make

#install learning code dependencies
cd ../../
sh install_dependencies.sh

# download data
cd data/
wget http://web3.cs.cornell.edu/pr/CAD-120/data/Subject1_rgbd_rawtext.tar.gz
wget http://pr.cs.cornell.edu/humanactivities/data/Subject1_annotations.tar.gz
tar -xvzf Subject1_annotations.tar.gz
tar -xvzf Subject1_rgbd_rawtext.tar.gz
mv  Subject1_rgbd_rawtext/*/*rgbd.txt  .
mkdir objects
mkdir objects_tracked
cp Subject1_annotations/*/objects/* objects/
cp Subject1_annotations/*/objects_tracked/* objects_tracked/
cp Subject1_annotations/*/*.bag .
cp Subject1_annotations/*/*.txt .
cat Subject1_annotations/*/activityLabel.txt  | grep -v END > activityLabel.txt
cat Subject1_annotations/*/labeling.txt > labeling.txt


# run anticipation code 
cd ../build/
./predict_seg ../data/ activityLabel.txt 1
