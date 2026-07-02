name: Backend CI

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    defaults:
      run:
        working-directory: backend

    steps:
      - name: Checkout Code
        uses: actions/checkout@v4

      - name: Setup Java
        uses: actions/setup-java@v4
        with:
          distribution: temurin
          java-version: '17'

      - name: Build Application
        run: mvn clean verify

      - name: Login to Docker Hub
        uses: docker/login-action@v3
        with:
          username: ${{ secrets.DOCKER_USERNAME }}
          password: ${{ secrets.DOCKER_PASSWORD }}

      - name: Build Docker Image
        run: |
          docker build -t ${{ secrets.DOCKER_USERNAME }}/insurance-backend:${{ github.sha }} .
          docker tag ${{ secrets.DOCKER_USERNAME }}/insurance-backend:${{ github.sha }} \
                     ${{ secrets.DOCKER_USERNAME }}/insurance-backend:latest

      - name: Push Docker Image
        run: |
          docker push ${{ secrets.DOCKER_USERNAME }}/insurance-backend:${{ github.sha }}
          docker push ${{ secrets.DOCKER_USERNAME }}/insurance-backend:latest