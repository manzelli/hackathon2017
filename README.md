# MalDetect

### What does it do?
MalDetect acts as a rudimentary malware detection system for Regin malware.

1. We train a k-means clustering model on two types of hashed packet captures: normal network traffic and Regin malware packets.
2. We continuously sniff the network for packets, periodically wrapping them into packet captures.
3. Using the trained model, we predict the likelihood of the live-captured packets being malicious.
4. The client is notified via SMS if malicious activity is detected on the network via Twilio.

### Executing the program
To execute the program, you will need the following Python libraries: 
- `scapy`
- `datasketch`
- `tensorflow`
- `numpy`

These can all be installed via `pip install`.

##### Training
`preprocess.py` - Preprocesses the training data. Returns a numpy array of hashed packet captures.

`TFKMeansCluster.py` - Trains the network using TensorFlow. Returns centroids and cluster assignments.

`categorize_clusters.py` - Determines which clusters contain normal and malicious packets. Calculates probabilities of normal vs. malicious traffic. Returns updated clusters.

`send_sms.py` - Twilio API request.

`send_packets.py` - Simulation of attack (sending a packet to another machine). Implementation not complete.

##### Pre-trained model
The objects from the pre-trained model are included as supporting files so that the user is able to run the program without collecting training data.

`normal-hashes.obj` & `malware-hashes.obj` - Contain the pickled hashes for the normal and malicious packet data. Normal data was captured from Wireshark, and malicious packet data was obtained at http://laredo-13.mit.edu/~brendan/regin/pcap/.

`centroids.obj` & `assignments.obj` - Contain the final centroids and the assignments of the hashes to the centroids.

##### Running
`test.py` - Live captures packets from the network and wraps them into packet captures. Predicts whether traffic is malicious by calculating the nearest centroid and estimating the probability of a correct prediction.
