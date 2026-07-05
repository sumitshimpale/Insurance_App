name: Deploy to Amazon EKS

on:
  workflow_dispatch:

jobs:

  deploy:

    runs-on: ubuntu-latest

    steps:

      - name: Checkout Repository
        uses: actions/checkout@v4

      - name: Configure AWS Credentials
        uses: aws-actions/configure-aws-credentials@v4
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: ${{ secrets.AWS_REGION }}

      - name: Configure kubeconfig
        run: |
          aws eks update-kubeconfig \
            --region ${{ secrets.AWS_REGION }} \
            --name ${{ secrets.EKS_CLUSTER_NAME }}

      - name: Verify Cluster
        run: kubectl get nodes

      - name: Deploy Namespace
        run: kubectl apply -f eks-insurance/00-namespace.yaml

      - name: Deploy StorageClass
        run: kubectl apply -f eks-insurance/01-storageclass.yaml

      - name: Deploy Mongo PVC
        run: kubectl apply -f eks-insurance/02-mongo-pvc.yaml

      - name: Deploy MongoDB
        run: kubectl apply -f eks-insurance/03-mongo.yaml

      - name: Deploy Backend
        run: kubectl apply -f eks-insurance/04-backend.yaml

      - name: Deploy Frontend
        run: kubectl apply -f eks-insurance/05-frontend.yaml

      - name: Deploy Ingress
        run: kubectl apply -f eks-insurance/06-ingress.yaml