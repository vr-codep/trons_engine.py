#!/usr/bin/env python3
import socket
import sys
import re
import urllib.request
import concurrent.futures
import time

# =================================================================
# TRONS X - FULL INTELLIGENCE SYSTEM
# PROPIEDAD DE: ANDRÉS GAVIRIA S.A.S. (vr-codep)
# =================================================================

class TronsXEngine:
    def __init__(self, domain):
        self.domain = domain
        self.subdomains = set()
        self.user_agent = 'Mozilla/5.0 (Windows NT 10.0; Win64; x64)'

    def banner(self):
        print("-" * 60)
        print("   SISTEMA DE INTELIGENCIA ESTRUCTURAL - TRONS X")
        print("   DESARROLLADO POR: ANDRÉS GAVIRIA S.A.S.")
        print("-" * 60)
        print(f"[*] OBJETIVO ESTRATÉGICO: {self.domain}\n")

    def query_crt_sh(self):
        """Rastreo en registros globales de certificados SSL"""
        print("[*] Extrayendo datos de certificados de seguridad...")
        url = f"https://crt.sh/?q=%.{self.domain}&output=json"
        try:
            req = urllib.request.Request(url, headers={'User-Agent': self.user_agent})
            with urllib.request.urlopen(req) as response:
                content = response.read().decode('utf-8')
                found = re.findall(r'"common_name":"([^"]+)"', content)
                for s in found:
                    if s.endswith(self.domain):
                        self.subdomains.add(s.lower().replace('*.', ''))
        except:
            print("[!] Aviso: Fuente de certificados fuera de línea temporalmente.")

    def brute_force_logic(self):
        """Inyecta subdominios críticos de la banca y servicios"""
        print("[*] Escaneando diccionarios de alta fidelidad...")
        # Estos son los que le interesan para Bancolombia y Bogotá
        critical_subs = [
            'portal', 'bancovirtual', 'sucursal', 'api', 'admin', 'secure', 
            'dev', 'm', 'pagos', 'transacciones', 'clientes', 'webmail',
            'vpn', 'movil', 'app', 'servicios', 'login', 'auth'
        ]
        for s in critical_subs:
            self.subdomains.add(f"{s}.{self.domain}")

    def probe(self, sub):
        """Verificación de vida y resolución de IP"""
        try:
            ip = socket.gethostbyname(sub)
            print(f"[+] DETECTADO: {sub.ljust(30)} | IP: {ip}")
            return (sub, ip)
        except:
            return None

    def start(self):
        self.banner()
        self.query_crt_sh()
        self.brute_force_logic()
        
        print(f"[*] Analizando {len(self.subdomains)} posibles puntos de acceso...")
        print("-" * 60)
        
        # Ejecución en paralelo (Multithreading de alta velocidad)
        with concurrent.futures.ThreadPoolExecutor(max_workers=15) as executor:
            executor.map(self.probe, list(self.subdomains))
        
        print("-" * 60)
        print("[!] Auditoría de la firma finalizada.")

if __name__ == "__main__":
    # Si no pones dominio, escanea el de tu interés por defecto
    target = sys.argv[1] if len(sys.argv) > 1 else "bancodebogota.com"
    engine = TronsXEngine(target)
    engine.start()
