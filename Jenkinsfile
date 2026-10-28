
stage('Install Python Dependencies') {
    steps {
        bat '''
            "C:\\Users\\hassi\\AppData\\Local\\Programs\\Python\\Python313\\python.exe" -m pip install -r requirements.txt
        '''
    }
}

stage('Run Tests') {
    steps {
        bat '''
            "C:\\Users\\hassi\\AppData\\Local\\Programs\\Python\\Python313\\python.exe" -m pytest -v
        '''
    }
}
