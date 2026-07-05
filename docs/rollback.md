name: Rollback Deployment

on:
  workflow_dispatch:
    inputs:
      image_tag:
        description: "Docker image tag to rollback to"
        required: true
        type: string

jobs:
  rollback:

    runs-on: ubuntu-latest

    steps:

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

      - name: Rollback Deployment
        run: |
          kubectl set image deployment/backend \
          backend=${{ secrets.DOCKER_USERNAME }}/insurance-backend:${{ github.event.inputs.image_tag }} \
          -n insurance

      - name: Verify Rollout
        run: |
          kubectl rollout status deployment/backend -n insurance